<script setup lang="ts">
import { library } from "@fortawesome/fontawesome-svg-core";
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";
import {
  faEdit,
  faRightFromBracket,
  faDeleteLeft,
  faEye,
} from "@fortawesome/free-solid-svg-icons";
import { onMounted, ref } from "vue";
import apiClient from "@/services/axios";
import { useRouter } from "vue-router";
import showToast from "@/components/showToast";
import { logout } from "@/router/auth";

const router = useRouter();

library.add(faEdit, faRightFromBracket, faDeleteLeft, faEye);

type Device = {
  id: string;
  datetime: string;
  amount: string;
  token: string;
};

type Schedule = {
  id: string;
  command: string;
  time: string;
  info: string;
};

function handleInput(event: Event) {
  const input = (event.target as HTMLInputElement).value;

  time.value = input
    .replace(/\D/g, "")
    .replace(/^(\d{2})(\d)/, "$1:$2")
    .replace(/:(\d{2})(\d)/, ":$1:$2")
    .slice(0, 8);
}

const isModalOpen = ref(false);

function openModal(selectedDevice: Device) {
  isModalOpen.value = true;
  device.value = selectedDevice;
}

const isDeviceModalOpen = ref(false);

function openDeviceModal(selectedDevice: Device) {
  isDeviceModalOpen.value = true;
  fetchSchedule(selectedDevice);
}

function closeModal() {
  isModalOpen.value = false;
  device.value = null;
}


function closeDeviceModal() {
  isDeviceModalOpen.value = false;
}

const devices = ref<Array<Device>>([]);
const device = ref<Device | null>(null);

const schedules = ref<Array<Schedule>>([]);

const command = ref<string>("");
const time = ref<string>("00:00:00");
const info = ref<string>("");

async function handleLogout() {
  const success = await logout();
  if (success) {
    router.push("/");
  }
}

async function addDevice() {
  const token = localStorage.getItem("authToken");
  await apiClient.post(`/api/device`, { name: "Novo Dispositivo", token })
    .then(response => {
      if (response.data["status"] !== "error") {
        showToast(response.data["message"], "success");
        fetchDevices();
      } else {
        showToast(response.data["message"]);
      }
    });
}


async function deleteDevice(device: Device) {
  const token = localStorage.getItem("authToken");

  await apiClient
    .delete(`/api/device?token=${device.token}`, {
      data: {
        token: token,
        deviceId: device.id,
      },
    })
    .then(async (response) => {
      if (response.data["status"] !== "error") {
        showToast(response.data["message"], "success");
        await fetchDevices();
      } else {
        showToast(response.data["message"]);
      }
    });
}

async function editDevice(
  deviceId: string,
  command: string,
  time: string,
  info?: string
) {
  const token = localStorage.getItem("authToken");
  await apiClient
    .post(`/api/schedule`, {
      token: token,
      deviceId: deviceId,
      command: command,
      time: time,
      info: info || null,
    })
    .then(async (response) => {
      if (response.data["status"] !== "error") {
        showToast(response.data["message"], "success");
        clearForm()
        closeModal();
        await fetchDevices();
      } else {
        showToast(response.data["message"]);
      }
    });
}

function clearForm() {
  command.value = ""
  time.value = "00:00:00"
  info.value = ""
}

async function fetchDevices() {
  const token = localStorage.getItem("authToken");

  await apiClient
    .get(`/api/user?token=${token}`)
    .then((response) => {
      if (response.data["status"] !== "error") {
        devices.value = response.data["devices"];
      } else {
        showToast(response.data["message"]);
      }
    })
    .catch(() => {
      showToast("Erro ao buscar dispositivos.");
    });
}

function fetchSchedule(selectedDevice: Device) {
  console.log(selectedDevice.id);
  console.log(selectedDevice.token);

  apiClient
    .get(`/api/device?token=${selectedDevice.token}`)
    .then(async (response) => {
      console.log(response);
      if (response.data["status"] !== "error") {
        schedules.value = response.data["schedule"];
        await fetchDevices();
      } else {
        showToast(response.data["message"]);
      }
    })
    .catch(() => {
      showToast("Erro ao buscar dispositivos.");
    });
}

onMounted(() => {
  fetchDevices();
});
</script>

<template>
  <div class="flex h-screen bg-gray-100 dashboard">
    <aside class="w-64 bg-bgcolor text-fontcolor flex flex-col">
      <div class="p-6 text-2xl font-bold">Dashboard</div>
      <nav class="flex flex-col space-y-4 p-6">
        <span
          to="/dashboard"
          readonly
          class="bg-gray-500 text-fontcolor p-5 rounded-full"
          >Devices</span
        >
      </nav>
      <button
        @click="handleLogout()"
        class="text-lg mb-4 mt-auto ml-auto mr-4 justify-end"
      >
        <span
          >Logout
          <font-awesome-icon class="ml-2" icon="right-from-bracket" />
        </span>
      </button>
    </aside>

    <main class="flex-col p-6 w-2/3 mx-auto">
      <section class="grid grid-cols-1 space-x-8 md:grid-cols-2 lg:grid-cols-3">
        <div class="bg-white p-6 rounded-lg shadow-md w-full">
          <h2 class="text-xl font-bold mb-2">Total de Dispositivos</h2>
          <p class="text-lg">
            {{ devices.length }}
          </p>
        </div>
        <button
          @click="addDevice"
          class="bg-bgcolor text-fontcolor p-6 rounded-lg shadow-md w-full"
        >
          <h2 class="text-xl font-bold">Adiconar Dispositivos</h2>
        </button>
      </section>

      <section>
        <div class="bg-white p-6 rounded-lg shadow-md">
          <h2 class="text-xl font-bold mb-4">Dispositivos</h2>
          <table
            class="min-w-full table-fixed text-center bg-fontcolor border border-gray-300"
          >
            <thead>
              <tr class="bg-gray-100">
                <th class="px-6 py-3 font-semibold">ID</th>
                <th class="px-6 py-3 font-semibold">Visualizar</th>
                <th class="px-6 py-3 font-semibold">Editar</th>
                <th class="px-6 py-3 font-semibold">Excluir</th>
                <th></th>
              </tr>
            </thead>
            <tbody>
              <tr
                v-for="device in devices"
                :key="device.id"
                class="hover:bg-gray-50"
              >
                <td class="px-6 py-4">{{ device.id }}</td>
                <td>
                  <button @click="openDeviceModal(device)">
                    <font-awesome-icon icon="eye" />
                  </button>
                </td>
                <td>
                  <button @click="openModal(device)">
                    <font-awesome-icon icon="edit" />
                  </button>
                </td>
                <td>
                  <button @click="deleteDevice(device)">
                    <font-awesome-icon icon="delete-left" />
                  </button>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </section>
    </main>
  </div>

  <div
    v-if="isModalOpen && device"
    class="fixed inset-0 flex items-center justify-center bg-black bg-opacity-75 z-50"
  >
    <div
      class="bg-gray-800 rounded-xl p-6 w-11/12 max-w-lg shadow-lg text-center text-white"
    >
      <button
        @click.stop="closeModal"
        class="ml-auto flex text-2xl text-gray-400 hover:text-white hover:cursor-pointer"
      >
        &times;
      </button>
      <h2 class="text-xl mb-4">Adicionar Agendamento</h2>
      <input
        placeholder="Comando"
        v-model="command"
        class="w-10/12 p-2 mb-4 bg-gray-700 rounded-lg outline-none text-white"
      />
      <input
        placeholder="Horário"
        v-model="time"
        maxlength="8"
        @input="handleInput"
        class="w-10/12 p-2 mb-4 bg-gray-700 rounded-lg outline-none text-white"
      />
      <input
        placeholder="Observação"
        v-model="info"
        class="w-10/12 p-2 mb-4 bg-gray-700 rounded-lg outline-none text-white"
      />
      <button
        type="submit"
        @click="editDevice(device.id, command, time, info)"
        class="bg-purple-500 hover:bg-purple-600 transition w-1/2 p-2 rounded-lg"
      >
        Editar
      </button>
    </div>
  </div>

  <div
    v-if="isDeviceModalOpen && schedules"
    class="fixed inset-0 flex items-center justify-center bg-black bg-opacity-75 z-50"
  >
    <div
      class="bg-gray-800 rounded-xl p-6 w-11/12 max-w-lg shadow-lg text-center text-white"
    >
      <button
        @click.stop="closeDeviceModal"
        class="ml-auto flex text-2xl text-gray-400 hover:text-white hover:cursor-pointer"
      >
        &times;
      </button>
      <table
        class="min-w-full table-fixed text-center mt-4 rounded-xl overflow-hidden"
      >
        <thead>
          <tr class="bg-purple-500">
            <th class="px-6 py-3 font-semibold">Comando</th>
            <th class="px-6 py-3 font-semibold">Horário</th>
            <th class="px-6 py-3 font-semibold">Observação</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="schedule in schedules" :key="schedule.id">
            <td class="px-6 py-4">{{ schedule.command }}</td>
            <td class="px-6 py-4">{{ schedule.time }}</td>
            <td class="px-6 py-4">{{ schedule.info }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<style lang="postcss">
.dashboard {
  @apply text-bgcolor h-full min-h-screen;
}
</style>
