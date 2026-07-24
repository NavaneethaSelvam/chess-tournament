<script>
  import {
    players,
    matches
  } from "$lib/data/store";


  const rankings = $derived(

    $players

      .map((player) => {

        const wins =
          $matches.filter(
            (match) =>
              String(match.winnerId) ===
              String(player.id)
          ).length;


        return {

          ...player,

          wins

        };

      })


      .sort(

        (a, b) =>
          b.wins - a.wins

      )


      .slice(0, 3)


      .map((player, index) => ({

        ...player,

        position:
          index + 1

      }))

  );

</script>


<div class="page-heading">

  <div>
<h2>
  🏆 Rankings
</h2>

    <p>
      Top 3 players based on tournament wins
    </p>

  </div>

</div>


<div class="ranking-card">


  {#if rankings.length === 0}


    <div class="empty-state">

      <div class="empty-icon">
        🏆
      </div>


      <h3>
        No Rankings Yet
      </h3>


      <p>
        Generate matches to see rankings.
      </p>

    </div>


  {:else}


    {#each rankings as player}


      <div class="ranking-row">


        <div class="position">


          {#if player.position === 1}

            🥇

          {:else if player.position === 2}

            🥈

          {:else}

            🥉

          {/if}


        </div>


        <div class="avatar">


          {player.name
            .charAt(0)
            .toUpperCase()}


        </div>


        <div class="player-info">


          <h3>
            {player.name}
          </h3>


          <p>
            Rating:
            {player.rating}
          </p>


        </div>


        <div class="wins">


          <strong>
            {player.wins}
          </strong>


          <span>
            Wins
          </span>


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


  .ranking-card {

    background: white;

    padding: 15px;

    border-radius: 18px;

    box-shadow:

      0 8px 25px

      rgba(15, 23, 42, 0.08);

  }


  .ranking-row {

    display: flex;

    align-items: center;

    gap: 20px;

    padding: 20px;

    border-bottom:

      1px solid

      #e5e7eb;

  }


  .ranking-row:last-child {

    border-bottom: none;

  }


  .position {

    font-size: 30px;

    width: 45px;

  }


  .avatar {

    width: 50px;

    height: 50px;

    border-radius: 50%;


    background:

      linear-gradient(

        135deg,

        #4f46e5,

        #8b5cf6

      );


    color: white;


    display: flex;

    align-items: center;

    justify-content: center;


    font-size: 20px;

    font-weight: bold;

  }


  .player-info {

    flex: 1;

  }


  .player-info h3 {

    margin: 0;

  }


  .player-info p {

    margin: 5px 0;

    color: #64748b;

  }


  .wins {

    display: flex;

    flex-direction: column;

    align-items: center;


    background: #eef2ff;


    padding:

      10px 20px;


    border-radius: 10px;

  }


  .wins strong {

    font-size: 22px;

    color: #4f46e5;

  }


  .wins span {

    font-size: 12px;

    color: #64748b;

  }


  .empty-state {

    text-align: center;

    padding: 50px;

    color: #64748b;

  }


  .empty-icon {

    font-size: 50px;

  }


</style>