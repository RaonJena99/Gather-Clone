<script>
  import { getDatabase, ref, onValue, set } from "firebase/database";
  import { onMount } from "svelte";

  const backDown = ["-3px -12px", "-50px -12px", "-3px -12px", "-140px -12px"];
  const backUp = ["-3px -80px", "-50px -82px", "-3px -80px", "-140px -82px"];
  const backLeft = [
    "-3px -149px",
    "-50px -150px",
    "-3px -149px",
    "-140px -150px",
  ];
  const backRight = [
    "-141px -218px",
    "-3px -220px",
    "-141px -218px",
    "-95px -220px",
  ];

  let top = 200;
  let left = 200;
  let backgroundPosition = backDown[0];
  let UpPos = 0;
  let downPos = 0;
  let LeftPos = 0;
  let RightPos = 0;

  async function onKeyDown(e) {
    switch (e.keyCode) {
      case 38:
        if (top - 25 >= 125) top -= 25;
        backgroundPosition = backUp[UpPos++ % 4];
        break;
      case 40:
        if (top + 25 <= 675) top += 25;
        backgroundPosition = backDown[downPos++ % 4];
        break;
      case 37:
        if (left - 25 >= 75) left -= 25;
        backgroundPosition = backLeft[LeftPos++ % 4];
        break;
      case 39:
        if (left + 25 <= 1325) left += 25;
        backgroundPosition = backRight[RightPos++ % 4];
        break;
    }
    await writeUserData("1", "player1", top, left);
    await readUserData();
  }

  function writeUserData(userId, name, idx_x, idx_y) {
    const db = getDatabase();
    set(ref(db, "users/" + userId), {
      name,
      idx_x,
      idx_y,
    });
  }

  function readUserData() {
    const db = getDatabase();
    const userDataRef = ref(db, "users/" + 1);
    onValue(userDataRef, (snapshot) => {
      const data = snapshot.val();
      players = [data];
    });
  }

  let players = [];
  onMount(() => {
    writeUserData("1", "player1", top, left), readUserData();
  });
</script>

<main>
  <div
    class="player"
    style="left: {left}px; top: {top}px;
    background-position:
    {backgroundPosition}"
  ></div>
</main>

{#each players as player}
  <div class="index_x">x: {player.idx_x}</div>
  <div class="index_y">y: {player.idx_y}</div>
{/each}

<svelte:window on:keydown|preventDefault={onKeyDown} />

<style>
  main {
    width: 100vw;
    height: 100vh;
    position: relative;
    background-image: url(../assets/Background.jpeg);
    background-size: 100vw 100vh;
  }

  .player {
    width: 40px;
    height: 50px;
    position: absolute;
    background-image: url(../assets/Character.png);
    background-size: 184px 275px;
    background-repeat: no-repeat;
  }

  .index_x {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 200px;
    height: 80px;
    position: absolute;
    bottom: 20%;
    left: 5%;
    background-image: url(../assets/Index.png);
    background-size: 200px 80px;
    font-family: cursive;
    font-weight: bold;
    font-size: 19px;
  }
  .index_y {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 200px;
    height: 80px;
    position: absolute;
    bottom: 10%;
    left: 5%;
    background-image: url(../assets/Index.png);
    background-size: 200px 80px;
    font-family: cursive;
    font-weight: bold;
    font-size: 19px;
  }
</style>
