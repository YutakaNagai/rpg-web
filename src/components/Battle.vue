<script setup>
import { ref, reactive } from "vue";

const props = defineProps({
  player: Object,
  enemy: Object,
});

const state = reactive({
  playerSpdProg: 0,
  enemySpdProg: 0,
  turn: 0,
  action: "",
  inBattle: false,
  isBattleEnd: false,
});

const messageList = reactive([{ message: "戦闘開始", color: "yellowgreen" }]);

const nextMessage = (message, color) => {
  messageList.push({ message, color });
};

const battleStart = () => {
  if (!state.isBattleEnd) {
    state.inBattle = true;
    const battle = setInterval(() => {
      if (!state.inBattle) {
        clearInterval(battle);
      } else {
        chkSpd();
      }
    }, 500);
  }
};

const battleStop = () => {
  state.inBattle = false;
};

const battleNext = () => {
  if (!state.isBattleEnd) {
    chkSpd();
  }
};

const battleSkip = () => {
  if (!state.isBattleEnd) {
    if (state.turn <= 100) {
      state.inBattle = true;
      const battle = setInterval(() => {
        if (!state.inBattle) {
          clearInterval(battle);
        } else {
          chkSpd();
        }
      }, 50);
    }
  }
};

const chkSpd = () => {
  state.action = "";
  state.turn++;
  while (!state.action) {
    state.playerSpdProg += Number(props.player.spd);
    if (state.playerSpdProg >= 100) {
      state.action = "player";
      state.playerSpdProg -= 100;
      const color = "skyblue";
      nextMessage("プレイヤーのターン", color);
      turnAction(props.player, props.enemy, color);
      break;
    }
    state.enemySpdProg += Number(props.enemy.spd);
    if (state.enemySpdProg >= 100) {
      state.action = "enemy";
      state.enemySpdProg -= 100;
      const color = "pink";
      nextMessage("エネミーのターン", color);
      turnAction(props.enemy, props.player, color);
      break;
    }
  }
};

const turnAction = (offense, defense, color) => {
  const damage = offense.atk - defense.def;
  defense.hp -= damage;
  nextMessage(
    `${offense.name}の攻撃！ ${defense.name}に${damage}のダメージ！`,
    color
  );
  if (defense.hp <= 0) {
    nextMessage(`${defense.name}は倒れた！`, "yellow");
    state.inBattle = false;
    state.isBattleEnd = true;
  }
};

// 現在のHPを初期化
props.player.hp = props.player.mhp;
props.enemy.hp = props.enemy.mhp;
</script>

<template>
  <!-- ステータス表示 -->
  <div class="status_player">
    <div class="status_block">
      <div>名前: {{ props.player.name }}</div>
      <div>HP: {{ props.player.hp }} / {{ props.player.mhp }}</div>
      <progress :max="props.player.mhp" :value="props.player.hp"></progress>
      <div>攻撃: {{ props.player.atk }}</div>
      <div>防御: {{ props.player.def }}</div>
      <div>敏捷: {{ props.player.spd }}</div>
      <progress max="100" :value="state.playerSpdProg"></progress>
    </div>
  </div>
  <div class="status_enemy">
    <div class="status_block">
      <div>名前: {{ props.enemy.name }}</div>
      <div>HP: {{ props.enemy.hp }} / {{ props.enemy.mhp }}</div>
      <progress :max="props.enemy.mhp" :value="props.enemy.hp"></progress>
      <div>攻撃: {{ props.enemy.atk }}</div>
      <div>防御: {{ props.enemy.def }}</div>
      <div>敏捷: {{ props.enemy.spd }}</div>
      <progress max="100" :value="state.enemySpdProg"></progress>
    </div>
  </div>

  <div class="btn">
    <button @click="battleStop">停止</button>
    <button @click="battleStart">開始</button>
    <button @click="battleNext">次のターン</button>
    <button @click="battleSkip">スキップ</button>
  </div>

  <!-- メッセージウインドウ -->
  <div
    v-for="(message, index) in messageList"
    :key="index"
    class="message"
    :style="`background-color: ${message.color};`"
  >
    {{ message.message }}
  </div>
</template>

<style scoped>
.status_player {
  display: inline-block;
  width: 30%;
  background: skyblue;
  border-radius: 10%;
  text-align: left;
  padding: 5%;
  margin: 5%;
}

.status_enemy {
  display: inline-block;
  width: 30%;
  background: pink;
  border-radius: 10%;
  text-align: left;
  padding: 5%;
  margin: 5%;
}

.status_block {
  text-align: left;
}

.btn {
  display: flex;
  justify-content: space-between;
  margin: 0px auto 20px;
  width: 60%;
}

.message {
  border: 1px solid black;
  height: 5%;
}
</style>
