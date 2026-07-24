<script>
  let totalPlayers = 0;
  let totalTournaments = 0;
  let totalMatches = 0;
</script>


<div class="page-heading">

  <div>

    <h2>
      Dashboard
    </h2>

    <p>
      Welcome to Chess Arena Tournament System
    </p>

  </div>

</div>


<div class="dashboard-grid">


  <div class="dashboard-card">

    <div class="card-icon">
      ♟️
    </div>

    <h3>
      Total Players
    </h3>

    <strong>
      {totalPlayers}
    </strong>

  </div>


  <div class="dashboard-card">

    <div class="card-icon">
      🏆
    </div>

    <h3>
      Total Tournaments
    </h3>

    <strong>
      {totalTournaments}
    </strong>

  </div>


  <div class="dashboard-card">

    <div class="card-icon">
      ⚔️
    </div>

    <h3>
      Total Matches
    </h3>

    <strong>
      {totalMatches}
    </strong>

  </div>


</div>


<div class="welcome-card">

  <div>

    <h2>
      Welcome to Chess Arena 👋
    </h2>

    <p>
      Manage your players, create tournaments,
      generate matches and view rankings.
    </p>

  </div>

  <span>
    ♛
  </span>

</div>


<style>

  .page-heading {

    margin-bottom: 30px;

  }


  .page-heading h2 {

    margin: 0;

    font-size: 30px;

    color:
      #111827;

  }


  .page-heading p {

    margin-top: 8px;

    color:
      #64748b;

  }


  .dashboard-grid {

    display: grid;

    grid-template-columns:
      repeat(
        auto-fit,
        minmax(220px, 1fr)
      );

    gap: 22px;

  }


  .dashboard-card {

    background: white;

    padding: 25px;

    border-radius: 18px;

    box-shadow:
      0 8px 25px
      rgba(15,23,42,0.08);

    transition:
      0.3s;

  }


  .dashboard-card:hover {

    transform:
      translateY(-5px);

  }


  .card-icon {

    font-size: 32px;

    margin-bottom: 15px;

  }


  .dashboard-card h3 {

    color:
      #64748b;

    margin:
      0 0 10px;

  }


  .dashboard-card strong {

    font-size: 32px;

    color:
      #4f46e5;

  }


  .welcome-card {

    margin-top: 30px;

    padding: 30px;

    border-radius: 18px;

    background:
      linear-gradient(
        135deg,
        #4f46e5,
        #7c3aed
      );

    color: white;

    display: flex;

    justify-content: space-between;

    align-items: center;

  }


  .welcome-card h2 {

    margin-top: 0;

  }


  .welcome-card p {

    opacity:
      0.85;

  }


  .welcome-card span {

    font-size: 80px;

  }

</style>