<script>
  import { onMount } from "svelte";
  import axios from "axios";

  let isLoggedin = true;
  let password = "123";
  let userPassword = "";

  onMount(async () => {
    const { data } = await axios.get("http://localhost:5000/getvals");
    if (data) {
      items = data;
    }
  });
  let items = [
    {
      title: "This is and insurance",
      desc: "           Lorem ipsum dolor sit amet consectetur adipisicing elit. Voluptates, provident. Porro placeat similique, quae dolores tempore neque quibusdam vitae architecto. Quia fugiat assumenda soluta quasi accusamus maiores eaque dolores sed.",
      name: "name",
      status: "pending",
    },
  ];

  function login() {
    if (userPassword === password) return (isLoggedin = true);
    alert("Wrong password");
  }
  async function accept(lid) {
    await axios.post("http://localhost:5000/accept", { lid });
    const { data } = await axios.get("http://localhost:5000/getvals");
    if (data) {
      items = data;
    }
  }
  async function reject(lid) {
    await axios.post("http://localhost:5000/reject", { lid });
    const { data } = await axios.get("http://localhost:5000/getvals");
    if (data) {
      items = data;
    }
  }
</script>

<div
  class="w-screen bg-slate-100 h-screen flex flex-col justify-center items-center"
>
  {#if !isLoggedin}
    <div class="p-10 shadow-md flex flex-col gap-3 rounded-lg w-fit bg-white">
      <div class="text-xl">Login to Admin portal</div>
      <form on:submit|preventDefault={login}>
        <input
          bind:value={userPassword}
          type="text"
          class="input input-bordered"
          name=""
          id=""
        />
        <button class="btn">Submit</button>
      </form>
    </div>
  {:else}
    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4 px-10 ">
      {#each items as item}
        <div class="bg-white p-10 rounded-xl shadow-lg">
          <div class="text-3xl font-bold">{item.title}</div>
          <div>{item.desc}</div>
          <div class="font-bold mt-2 opacity-50">{item.status}</div>
          <div class="flex gap-3 mt-5">
            <!-- svelte-ignore a11y-click-events-have-key-events -->
            <div
              on:click={() => {
                accept(item.lid);
              }}
              class="btn btn-outline"
            >
              Accept
            </div>
            <!-- svelte-ignore a11y-click-events-have-key-events -->
            <div
              on:click={() => {
                reject(item.lid);
              }}
              class="btn btn-outline"
            >
              Reject
            </div>
          </div>
        </div>
      {/each}
    </div>
  {/if}
</div>
