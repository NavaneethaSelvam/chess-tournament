<script>

  import {
    players,
    tournaments
  } from "$lib/data/store";


  let tournamentName = "";

  let tournamentLocation = "";

  let tournamentDate = "";

  let editingTournamentId = null;

  let selectedTournamentId = "";


  // =========================
  // ADD / UPDATE TOURNAMENT
  // =========================

  function addTournament() {

    if (
      !tournamentName.trim() ||
      !tournamentLocation.trim() ||
      !tournamentDate
    ) {

      alert("Please fill all fields");

      return;

    }


    // UPDATE TOURNAMENT
    if (editingTournamentId !== null) {

      tournaments.update(
        (currentTournaments) => {

          return currentTournaments.map(
            (tournament) => {

              if (
                String(tournament.id) ===
                String(editingTournamentId)
              ) {

                return {

                  ...tournament,

                  name:
                    tournamentName.trim(),

                  location:
                    tournamentLocation.trim(),

                  date:
                    tournamentDate

                };

              }


              return tournament;

            }

          );

        }

      );


      alert(
        "Tournament updated successfully!"
      );

    }


    // ADD NEW TOURNAMENT
    else {

      const newTournament = {

        id: Date.now(),

        name:
          tournamentName.trim(),

        location:
          tournamentLocation.trim(),

        date:
          tournamentDate,

        players: []

      };


      tournaments.update(
        (currentTournaments) => [

          ...currentTournaments,

          newTournament

        ]

      );


      alert(
        "Tournament added successfully!"
      );

    }


    clearForm();

  }


  // =========================
  // EDIT TOURNAMENT
  // =========================

  function editTournament(tournament) {

    editingTournamentId =
      tournament.id;


    tournamentName =
      tournament.name;


    tournamentLocation =
      tournament.location;


    tournamentDate =
      tournament.date;

  }


  // =========================
  // DELETE TOURNAMENT
  // =========================

  function deleteTournament(id) {

    tournaments.update(
      (currentTournaments) => {

        return currentTournaments.filter(
          (tournament) =>

            tournament.id !== id

        );

      }

    );


    // Deleted tournament selected
    if (
      String(selectedTournamentId) ===
      String(id)
    ) {

      selectedTournamentId = "";

    }

  }


  // =========================
  // ADD PLAYER TO TOURNAMENT
  // =========================

  function addPlayerToTournament(playerId) {

    // Tournament select பண்ணியிருக்கிறோமா?
    if (!selectedTournamentId) {

      alert(
        "Please select a tournament first"
      );

      return;

    }


    tournaments.update(
      (currentTournaments) => {

        return currentTournaments.map(
          (tournament) => {


            // Selected tournament
            if (
              String(tournament.id) ===
              String(selectedTournamentId)
            ) {


              // Existing players
              const currentPlayers =
                tournament.players || [];


              // Already added check
              const alreadyAdded =
                currentPlayers.some(
                  (id) =>

                    String(id) ===
                    String(playerId)

                );


              if (alreadyAdded) {

                alert(
                  "Player already added to this tournament"
                );

                return tournament;

              }


              // ADD PLAYER
              return {

                ...tournament,

                players: [

                  ...currentPlayers,

                  Number(playerId)

                ]

              };

            }


            return tournament;

          }

        );

      }

    );


    alert(
      "Player added successfully!"
    );

  }


  // =========================
  // CLEAR FORM
  // =========================

  function clearForm() {

    tournamentName = "";

    tournamentLocation = "";

    tournamentDate = "";

    editingTournamentId = null;

  }

</script>

<div class="page-heading">

  <div>

    <h2>🏆 Tournaments</h2>

<p>Manage tournaments and registered players</p>

  </div>

</div>


<!-- TOURNAMENT FORM -->

<div class="form-card">

  <input
    type="text"
    placeholder="Tournament Name"
    bind:value={tournamentName}
  />


  <input
    type="text"
    placeholder="Location"
    bind:value={
      tournamentLocation
    }
  />


  <input
    type="date"
    bind:value={tournamentDate}
  />


  <button
    class="primary-btn"
    on:click={addTournament}
  >

    {editingTournamentId !== null
      ? "Update Tournament"
      : "Add Tournament"}

  </button>

</div>


<!-- SELECT TOURNAMENT -->

<div class="select-card">

  <h3>
    👥 Add Players to Tournament
  </h3>


  <select
    bind:value={selectedTournamentId}
  >

    <option value="">
      Select Tournament
    </option>


    {#each $tournaments as tournament}

      <option value={tournament.id}>

        {tournament.name}

      </option>

    {/each}

  </select>

</div>


<!-- PLAYERS -->

<div class="players-grid">

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
        Rating: {player.rating}
      </p>


      <button
        class="add-btn"
        on:click={() =>
          addPlayerToTournament(
            player.id
          )}
      >

        ➕ Add to Tournament

      </button>

    </div>

  {/each}

</div>


<!-- TOURNAMENT LIST -->

<h2 class="section-title">
  All Tournaments
</h2>


<div class="tournaments-grid">

  {#if $tournaments.length === 0}

    <div class="empty-state">

      🏆

      <h3>
        No tournaments yet
      </h3>

    </div>

  {:else}

    {#each $tournaments as tournament}

      <div class="tournament-card">

        <div class="tournament-icon">
          🏆
        </div>


        <h3>
          {tournament.name}
        </h3>


        <p>
          📍 {tournament.location}
        </p>


        <p>
          📅 {tournament.date}
        </p>


        <p class="player-count">

          👥 Players:
          {tournament.players.length}

        </p>


        <div class="actions">

          <button
            class="edit-btn"
            on:click={() =>
              editTournament(
                tournament
              )}
          >

            ✏️ Edit

          </button>


          <button
            class="delete-btn"
            on:click={() =>
              deleteTournament(
                tournament.id
              )}
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

  }


  .page-heading p {

    color: #64748b;

  }


  .form-card {

    background: white;

    padding: 25px;

    border-radius: 18px;

    display: flex;

    gap: 14px;

    flex-wrap: wrap;

    margin-bottom: 25px;

    box-shadow:
      0 8px 25px
      rgba(15, 23, 42, 0.08);

  }


  input {

    flex: 1;

    min-width: 200px;

    padding: 14px;

    border: 1px solid #d1d5db;

    border-radius: 10px;

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


  .select-card {

    background: white;

    padding: 22px;

    border-radius: 18px;

    margin-bottom: 25px;

    box-shadow:
      0 8px 25px
      rgba(15, 23, 42, 0.06);

  }


  .select-card h3 {

    margin-top: 0;

  }


  select {

    width: 100%;

    padding: 13px;

    border-radius: 10px;

    border: 1px solid #d1d5db;

  }


  .players-grid {

    display: grid;

    grid-template-columns:
      repeat(
        auto-fill,
        minmax(220px, 1fr)
      );

    gap: 20px;

  }


  .player-card {

    background: white;

    padding: 22px;

    border-radius: 18px;

    text-align: center;

    box-shadow:
      0 8px 25px
      rgba(15, 23, 42, 0.06);

  }


  .player-avatar {

    width: 60px;

    height: 60px;

    margin: auto;

    display: flex;

    align-items: center;

    justify-content: center;

    background:
      linear-gradient(
        135deg,
        #4f46e5,
        #8b5cf6
      );

    color: white;

    border-radius: 50%;

    font-size: 24px;

    font-weight: bold;

  }


  .add-btn {

    border: none;

    background: #dcfce7;

    color: #15803d;

    padding: 10px 14px;

    border-radius: 8px;

    cursor: pointer;

    font-weight: 600;

  }


  .section-title {

    margin-top: 35px;

  }


  .tournaments-grid {

    display: grid;

    grid-template-columns:
      repeat(
        auto-fill,
        minmax(260px, 1fr)
      );

    gap: 22px;

  }


  .tournament-card {

    background: white;

    padding: 25px;

    border-radius: 18px;

    border: 1px solid #e5e7eb;

    box-shadow:
      0 8px 25px
      rgba(15, 23, 42, 0.06);

  }


  .tournament-icon {

    font-size: 40px;

  }


  .tournament-card p {

    color: #64748b;

  }


  .player-count {

    color: #4f46e5 !important;

    font-weight: 600;

  }


  .actions {

    margin-top: 20px;

  }


  .edit-btn {

    border: none;

    background: #e0e7ff;

    color: #4338ca;

    padding: 9px 16px;

    border-radius: 8px;

    cursor: pointer;

    margin-right: 8px;

  }


  .delete-btn {

    border: none;

    background: #fee2e2;

    color: #dc2626;

    padding: 9px 16px;

    border-radius: 8px;

    cursor: pointer;

  }


  .empty-state {

    background: white;

    padding: 40px;

    border-radius: 18px;

    text-align: center;

  }

</style>