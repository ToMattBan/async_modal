<template>
  <main>
    <section class="_o-wrapper">
      <h1 class="_tac">Male Basketball Players "BAKBL Club"</h1>
      <form @submit.prevent="addNewPlayer">
        <table class="_1/1">
          <thead>
            <tr>
              <th>Name</th>
              <th>Surname</th>
              <th>Age</th>
              <th>Country</th>
              <th>Games</th>
              <th>Points</th>
              <th></th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(player, index) in playersData">
              <td>{{ player.name }}</td>
              <td>{{ player.surname }}</td>
              <td>{{ player.age }}</td>
              <td>{{ player.country }}</td>
              <td>{{ player.games }}</td>
              <td>{{ player.points }}</td>
              <td class="_cp" @click="removePlayer(index)">✖️</td>
            </tr>

            <tr class="newPlayer">
              <td><input required v-model="newPlayerName" /></td>
              <td><input required v-model="newPlayerSurname" /></td>
              <td><input required v-model="newPlayerAge" type="number" inputmode="numeric" /></td>
              <td><input required v-model="newPlayerCountry" /></td>
              <td><input required v-model="newPlayerGames" type="number" inputmode="numeric" /></td>
              <td><input required v-model="newPlayerPoints" type="number" inputmode="numeric" /></td>
              <td><button type="submit" class="actionBtn">➕</button></td>
            </tr>
          </tbody>
        </table>
      </form>

      <Modal ref="modalAddPlayer" />
    </section>
  </main>
</template>

<script lang="ts">
import { defineComponent, ref } from "vue";
import initialData from "./data.json";
import Modal from "./components/Modal.vue";

interface IPlayersData {
  name: string;
  surname: string;
  age: number;
  country: string;
  games: number;
  points: number;
}

export default defineComponent({
  setup() {
    const modalAddPlayer = ref();
    const playersData = ref<IPlayersData[]>(initialData);
    const newPlayerName = ref<string | null>(null);
    const newPlayerSurname = ref<string | null>(null);
    const newPlayerAge = ref<number | null>(null);
    const newPlayerCountry = ref<string | null>(null);
    const newPlayerGames = ref<number | null>(null);
    const newPlayerPoints = ref<number | null>(null);

    function addNewPlayer() {
      const newPlayer: IPlayersData = {
        name: newPlayerName.value ?? "",
        surname: newPlayerSurname.value ?? "",
        age: newPlayerAge.value ?? 0,
        country: newPlayerCountry.value ?? "",
        games: newPlayerGames.value ?? 0,
        points: newPlayerPoints.value ?? 0,
      };

      playersData.value.push(newPlayer);

      newPlayerName.value = null;
      newPlayerSurname.value = null;
      newPlayerAge.value = null;
      newPlayerCountry.value = null;
      newPlayerGames.value = null;
      newPlayerPoints.value = null;
    }

    async function removePlayer(index: number) {
      await modalAddPlayer.value.abrir().then(() => {
        playersData.value.splice(index, 1);
      });
    }

    return {
      modalAddPlayer,
      playersData,
      newPlayerName,
      newPlayerSurname,
      newPlayerAge,
      newPlayerCountry,
      newPlayerGames,
      newPlayerPoints,
      addNewPlayer,
      removePlayer,
    };
  },
  components: { Modal },
});
</script>

<style scoped lang="scss">
table {
  th,
  td {
    text-align: center;
  }

  tbody {
    tr:nth-child(odd) {
      background-color: #ece7e7;
    }

    td {
      padding: 8px;
    }
    .actionBtn {
      background-color: transparent;
      border: none;
      font-size: 16px;
      cursor: pointer;
    }

    .newPlayer {
      input {
        width: 100%;
      }
    }
  }
}
</style>
