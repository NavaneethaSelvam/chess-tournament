<script>
  import { page } from "$app/state";

  let { children } = $props();

  let menuItems = [
    {
      name: "Dashboard",
      path: "/",
      icon: "📊"
    },
    {
      name: "Players",
      path: "/players",
      icon: "♟️"
    },
    {
      name: "Tournaments",
      path: "/tournaments",
      icon: "🏆"
    },
    {
      name: "Matches",
      path: "/matches",
      icon: "⚔️"
    },
    {
      name: "Rankings",
      path: "/rankings",
      icon: "🥇"
    }
  ];
</script>

<div class="app">

  <!-- SIDEBAR -->

  <aside class="sidebar">

    <div class="brand">

      <div class="logo">
        ♟
      </div>

      <div>
        <h2>Chess Arena</h2>
        <span>Tournament System</span>
      </div>

    </div>


    <nav class="sidebar-nav">

      <p class="menu-title">
        MAIN MENU
      </p>


      {#each menuItems as item}

        <a
          href={item.path}
          class:active={page.url.pathname === item.path}
          class="nav-item"
        >

          <span class="nav-icon">
            {item.icon}
          </span>

          <span>
            {item.name}
          </span>

        </a>

      {/each}

    </nav>


    <div class="sidebar-footer">

      <div class="status-dot"></div>

      <span>
        System Online
      </span>

    </div>

  </aside>


  <!-- MAIN AREA -->

  <div class="main-area">


    <!-- TOPBAR -->

    <header class="topbar">

      <div>

        <h1>
          Chess Tournament Management
        </h1>

        <p>
          Manage players, tournaments and matches
        </p>

      </div>


      <div class="profile">

        <div class="profile-icon">
          👤
        </div>

        <div>

          <strong>
            Admin
          </strong>

          <small>
            Tournament Manager
          </small>

        </div>

      </div>

    </header>


    <!-- PAGE CONTENT -->

    <main class="content">

      {@render children()}

    </main>


  </div>

</div>


<style>

  :global(*) {
    box-sizing: border-box;
  }


  :global(body) {

    margin: 0;

    font-family:
      Inter,
      Arial,
      sans-serif;

    background:
      #f5f7fb;

    color:
      #1e293b;

  }


  .app {

    display: flex;

    min-height: 100vh;

  }


  /* SIDEBAR */

  .sidebar {

    width: 260px;

    min-height: 100vh;

    background:
      linear-gradient(
        180deg,
        #111827,
        #1e1b4b
      );

    color: white;

    padding: 25px 18px;

    display: flex;

    flex-direction: column;

  }


  .brand {

    display: flex;

    align-items: center;

    gap: 12px;

    padding:
      0 12px 30px;

    border-bottom:
      1px solid
      rgba(255,255,255,0.1);

  }


  .logo {

    width: 45px;

    height: 45px;

    display: flex;

    align-items: center;

    justify-content: center;

    background:
      linear-gradient(
        135deg,
        #6366f1,
        #8b5cf6
      );

    border-radius: 12px;

    font-size: 25px;

  }


  .brand h2 {

    margin: 0;

    font-size: 17px;

  }


  .brand span {

    font-size: 11px;

    color:
      #a5b4fc;

  }


  /* NAVIGATION */

  .sidebar-nav {

    margin-top: 30px;

  }


  .menu-title {

    font-size: 11px;

    color:
      #94a3b8;

    padding:
      0 14px;

    letter-spacing: 1px;

  }


  .nav-item {

    display: flex;

    align-items: center;

    gap: 14px;

    padding:
      14px 16px;

    margin:
      8px 0;

    color:
      #cbd5e1;

    text-decoration: none;

    border-radius: 12px;

    transition:
      0.3s;

  }


  .nav-item:hover {

    background:
      rgba(255,255,255,0.08);

    color: white;

    transform:
      translateX(4px);

  }


  .nav-item.active {

    background:
      linear-gradient(
        135deg,
        #6366f1,
        #8b5cf6
      );

    color: white;

    box-shadow:
      0 8px 20px
      rgba(99,102,241,0.3);

  }


  .nav-icon {

    width: 25px;

    text-align: center;

    font-size: 20px;

  }


  /* FOOTER */

  .sidebar-footer {

    margin-top: auto;

    display: flex;

    align-items: center;

    gap: 8px;

    padding: 15px;

    color:
      #94a3b8;

    font-size: 13px;

  }


  .status-dot {

    width: 9px;

    height: 9px;

    background:
      #22c55e;

    border-radius: 50%;

  }


  /* MAIN */

  .main-area {

    flex: 1;

    min-width: 0;

  }


  /* TOPBAR */

  .topbar {

    background: white;

    padding:
      24px 35px;

    display: flex;

    align-items: center;

    justify-content: space-between;

    border-bottom:
      1px solid
      #e5e7eb;

  }


  .topbar h1 {

    margin: 0;

    font-size: 24px;

    color:
      #111827;

  }


  .topbar p {

    margin:
      6px 0 0;

    color:
      #64748b;

    font-size: 14px;

  }


  .profile {

    display: flex;

    align-items: center;

    gap: 10px;

  }


  .profile-icon {

    width: 40px;

    height: 40px;

    display: flex;

    align-items: center;

    justify-content: center;

    background:
      #eef2ff;

    border-radius: 50%;

    font-size: 20px;

  }


  .profile strong {

    display: block;

    font-size: 14px;

  }


  .profile small {

    color:
      #64748b;

  }


  /* CONTENT */

  .content {

    padding:
      35px;

  }


  /* MOBILE */

  @media (max-width: 768px) {

    .sidebar {

      width: 75px;

      padding:
        20px 10px;

    }


    .brand div:not(.logo),

    .nav-item span:not(.nav-icon),

    .menu-title,

    .sidebar-footer span {

      display: none;

    }


    .brand {

      justify-content: center;

    }


    .nav-item {

      justify-content: center;

    }


    .topbar {

      padding:
        20px;

    }


    .topbar h1 {

      font-size: 18px;

    }


    .profile {

      display: none;

    }


    .content {

      padding:
        20px;

    }

  }

</style>