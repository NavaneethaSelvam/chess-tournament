<script>
  import { players } from "$lib/data/store";


  let playerName = "";

  let playerEmail = "";

  let playerRating = "";

  let editingPlayerId = null;


  function addPlayer() {

    if (
      !playerName ||
      !playerEmail ||
      !playerRating
    ) {

      alert("Please fill all fields");

      return;

    }


    if (editingPlayerId !== null) {

      players.update((currentPlayers) => {

        return currentPlayers.map((player) => {

          if (
            player.id === editingPlayerId
          ) {

            return {

              ...player,

              name: playerName,

              email: playerEmail,

              rating: Number(playerRating)

            };

          }

          return player;

        });

      });


      editingPlayerId = null;

    } else {

      players.update((currentPlayers) => [

        ...currentPlayers,

        {

          id: Date.now(),

          name: playerName,

          email: playerEmail,

          rating: Number(playerRating),

          wins: 0

        }

      ]);

    }


    clearForm();

  }


  function editPlayer(player) {

    editingPlayerId = player.id;

    playerName = player.name;

    playerEmail = player.email;

    playerRating = player.rating;

  }


  function deletePlayer(id) {

    players.update((currentPlayers) =>

      currentPlayers.filter(

        (player) => player.id !== id

      )

    );

  }


  function clearForm() {

    playerName = "";

    playerEmail = "";

    playerRating = "";

    editingPlayerId = null;

  }

</script>


<div class="page-heading">

  <div>

    <h2>♟️ Players</h2>

    <p>
      Manage all registered chess players
    </p>

  </div>

</div>


<div class="form-card">

  <input
    type="text"
    placeholder="Player Name"
    bind:value={playerName}
  />


  <input
    type="email"
    placeholder="Email"
    bind:value={playerEmail}
  />


  <input
    type="number"
    placeholder="Rating"
    bind:value={playerRating}
  />


  <button
    class="primary-btn"
    on:click={addPlayer}
  >

    {editingPlayerId !== null
      ? "Update Player"
      : "Add Player"}

  </button>

</div>


<div class="players-grid">

  {#if $players.length === 0}

    <div class="empty-state">

      ♟️

      <h3>
        No players yet
      </h3>

      <p>
        Add your first chess player
      </p>

    </div>

  {:else}

    {#each $players as player}

      <div class="player-card">

        <div class="player-avatar">

          {player.name
            .charAt(0)
            .toUpperCase()}

        </div>


        <h3>
          {player.name}
        </h3>


        <p>
          {player.email}
        </p>


        <span class="rating">

          Rating: {player.rating}

        </span>


        <div class="actions">

          <button
            class="edit-btn"
            on:click={() =>
              editPlayer(player)}
          >

            ✏️ Edit

          </button>


          <button
            class="delete-btn"
            on:click={() =>
              deletePlayer(player.id)}
          >

            🗑️ Delete

          </button>

        </div>

      </div>

    {/each}

  {/if}

</div>


<style>

  .page-heading {

    margin-bottom: 30px;

  }


  .page-heading h2 {

    margin: 0;

    font-size: 30px;

    color: #111827;

  }


  .page-heading p {

    color: #64748b;

    margin-top: 8px;

  }


  .form-card {

    background: white;

    padding: 25px;

    border-radius: 18px;

    display: flex;

    gap: 14px;

    flex-wrap: wrap;

    margin-bottom: 30px;

    box-shadow:

      0 8px 25px

      rgba(15, 23, 42, 0.08);

  }


  input {

    flex: 1;

    min-width: 200px;

    padding: 14px 16px;

    border: 1px solid #d1d5db;

    border-radius: 10px;

    font-size: 14px;

    outline: none;

  }


  input:focus {

    border-color: #6366f1;

  }


  .primary-btn {

    background:

      linear-gradient(

        135deg,

        #4f46e5,

        #7c3aed

      );

    color: white;

    border: none;

    padding: 13px 20px;

    border-radius: 10px;

    cursor: pointer;

    font-weight: 600;

  }


  .players-grid {

    display: grid;

    grid-template-columns:

      repeat(

        auto-fill,

        minmax(250px, 1fr)

      );

    gap: 22px;

  }


  .player-card {

    background: white;

    padding: 25px;

    border-radius: 18px;

    text-align: center;

    border: 1px solid #e5e7eb;

    box-shadow:

      0 8px 25px

      rgba(15, 23, 42, 0.06);

  }


  .player-avatar {

    width: 70px;

    height: 70px;

    margin: auto;

    background:

      linear-gradient(

        135deg,

        #4f46e5,

        #8b5cf6

      );

    color: white;

    border-radius: 50%;

    display: flex;

    align-items: center;

    justify-content: center;

    font-size: 28px;

    font-weight: bold;

  }


  .player-card p {

    color: #64748b;

  }


  .rating {

    display: block;

    background: #eef2ff;

    color: #4f46e5;

    padding: 9px;

    border-radius: 8px;

    margin: 15px 0;

    font-weight: 600;

  }


  .actions {

    display: flex;

    justify-content: center;

    gap: 8px;

  }


  .edit-btn {

    border: none;

    background: #e0e7ff;

    color: #4338ca;

    padding: 9px 16px;

    border-radius: 8px;

    cursor: pointer;

    font-weight: 600;

  }


  .delete-btn {

    border: none;

    background: #fee2e2;

    color: #dc2626;

    padding: 9px 16px;

    border-radius: 8px;

    cursor: pointer;

    font-weight: 600;

  }


  .empty-state {

    background: white;

    padding: 50px;

    border-radius: 18px;

    text-align: center;

    color: #64748b;

  }

</style>