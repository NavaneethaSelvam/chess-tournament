<script>
  import {
    players,
    tournaments,
    matches
  } from "$lib/data/store";

  let selectedTournamentId = "";

  function generateMatches() {
    console.log("Generate button clicked");

    if (!selectedTournamentId) {
      alert("Please select a tournament");
      return;
    }

    const tournament = $tournaments.find(
      (tournament) =>
        String(tournament.id) === String(selectedTournamentId)
    );

    if (!tournament) {
      alert("Tournament not found");
      return;
    }

    const tournamentPlayerIds = tournament.players || [];

    if (tournamentPlayerIds.length < 2) {
      alert("At least 2 players are required in this tournament");
      return;
    }

    const tournamentPlayers = $players.filter((player) =>
      tournamentPlayerIds.some(
        (playerId) =>
          String(playerId) === String(player.id)
      )
    );

    if (tournamentPlayers.length < 2) {
      alert("Players could not be loaded correctly");
      return;
    }

    const shuffledPlayers = [...tournamentPlayers].sort(
      () => Math.random() - 0.5
    );

    const newMatches = [];

    for (
      let i = 0;
      i + 1 < shuffledPlayers.length;
      i += 2
    ) {
      const playerOne = shuffledPlayers[i];
      const playerTwo = shuffledPlayers[i + 1];

      const winner =
        Math.random() < 0.5
          ? playerOne
          : playerTwo;

     newMatches.push({

  id: Date.now() + i,

  tournamentId: tournament.id,

  tournamentName: tournament.name,

  playerOne: playerOne.name,

  playerTwo: playerTwo.name,

  winner: winner.name,


  winnerId: winner.id

});
    }

    matches.update((currentMatches) => [
      ...currentMatches,
      ...newMatches
    ]);

    alert(
      `${newMatches.length} match(es) generated successfully`
    );
  }

  function deleteMatch(id) {
    matches.update((currentMatches) =>
      currentMatches.filter(
        (match) => match.id !== id
      )
    );
  }
</script>


<div class="page-heading">

  <h2>⚔️ Matches</h2>

  <p>
    Generate random matches and record winners
  </p>

</div>


<div class="form-card">

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


  <button
    class="primary-btn"
    on:click={generateMatches}
  >

    ⚔️ Generate Random Matches

  </button>

</div>


<div class="matches-list">

 {#if $matches.length === 0}

    <div class="empty-state">

      <div class="empty-icon">
        ⚔️
      </div>

      <h3>
        No Matches Yet
      </h3>

      <p>
        Select a tournament and generate matches.
      </p>

    </div>

  {:else}

    {#each $matches as match}

      <div class="match-card">

        <h3>
          🏆 {match.tournamentName}
        </h3>


        <div class="players">

          <div class="player">

            <span>
              ♟️
            </span>

            <strong>
              {match.playerOne}
            </strong>

          </div>


          <div class="versus">
            VS
          </div>


          <div class="player">

            <span>
              ♟️
            </span>

            <strong>
              {match.playerTwo}
            </strong>

          </div>

        </div>


        <div class="winner">

          🏆 Winner:

          <strong>
            {match.winner}
          </strong>

        </div>


        <button
          class="delete-btn"
          on:click={() =>
            deleteMatch(match.id)}
        >

          🗑️ Delete

        </button>

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
    align-items: center;
    gap: 14px;
    flex-wrap: wrap;
    margin-bottom: 30px;
    box-shadow:
      0 8px 25px
      rgba(15, 23, 42, 0.08);
  }

  select {
    flex: 1;
    min-width: 250px;
    padding: 14px;
    border: 1px solid #d1d5db;
    border-radius: 10px;
    font-size: 15px;
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

  .primary-btn:hover {
    opacity: 0.9;
  }

  .match-card {
    background: white;
    padding: 25px;
    border-radius: 18px;
    margin-bottom: 20px;
    box-shadow:
      0 8px 25px
      rgba(15, 23, 42, 0.06);
  }

  .match-card h3 {
    margin-top: 0;
    color: #4f46e5;
  }

  .players {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 35px;
  }

  .player {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 8px;
  }

  .player span {
    font-size: 35px;
  }

  .versus {
    font-weight: bold;
    color: #6366f1;
  }

  .winner {
    margin-top: 20px;
    padding: 12px;
    background: #ecfdf5;
    color: #15803d;
    border-radius: 8px;
    text-align: center;
  }

  .delete-btn {
    margin-top: 15px;
    border: none;
    background: #fee2e2;
    color: #dc2626;
    padding: 9px 16px;
    border-radius: 8px;
    cursor: pointer;
  }

  .empty-state {
    background: white;
    padding: 50px;
    border-radius: 18px;
    text-align: center;
    color: #64748b;
  }

  .empty-icon {
    font-size: 50px;
  }

</style>