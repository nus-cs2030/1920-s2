<header>
  <navbar type="dark">
    <a slot="brand" href="{{baseUrl}}/index.html" title="Home" class="navbar-brand">CS2030 AY19/20 Semester 2</a>
    <dropdown text="Peer Learning" class="nav-link">
      <li><a href="{{baseUrl}}/contents/peerlearning/peerlearning.html" class="dropdown-item">Introduction</a></li>
    </dropdown>
    <dropdown text="Textbook" style="font-weight: bold;" class="nav-link">
    </dropdown>
    <li><a slot="brand" href="https://github.com/nus-cs-2030/ay1920-s2" class="nav-link">Github Link</a></li>
    <li slot="right">
      <form class="navbar-form">
        <searchbar :data="searchData" placeholder="Search" :on-hit="searchCallback" menu-align-right></searchbar>
      </form>
    </li>
  </navbar>
</header>