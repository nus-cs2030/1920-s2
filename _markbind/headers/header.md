<header>
  <navbar type="dark">
    <a slot="brand" href="{{baseUrl}}/index.html" title="Home" class="navbar-brand">CS2030 AY19/20 Semester 2</a>
    <dropdown text="Peer Learning" class="nav-link">
      <li><a href="{{baseUrl}}/contents/peerlearning/peerlearning.html" class="dropdown-item">Introduction</a></li>
      <li><a href="{{baseUrl}}/contents/peerlearning/textbook.html" class="dropdown-item">Textbook Contributions</a></li>
      <li><a href="{{baseUrl}}/contents/peerlearning/peerlearningtask.html" class="dropdown-item">Peer Learning Tasks</a></li>
      <li><a href="{{baseUrl}}/contents/peerlearning/piazza.html" class="dropdown-item">Piazza Participation</a></li>
    </dropdown>
    <dropdown text="Guides" class="nav-link">
      <li><a href="{{baseUrl}}/contents/guides/settingUpLabEnv.html" class="dropdown-item">Setting Up Lab Environment</a></li>
      <li><a href="{{baseUrl}}/contents/guides/settingUpJava.html" class="dropdown-item">Setting Up Java</a></li>
      <li><a href="{{baseUrl}}/contents/guides/settingUpVim.html" class="dropdown-item">Setting Up Vim</a></li>
    </dropdown>
    <li><a slot="brand" href="{{baseUrl}}/contents/textbook/textbook.html"class="nav-link">Textbook</a></li>
    <li><a slot="brand" href="https://github.com/nus-cs-2030/ay1920-s2" class="nav-link">Github Repo</a></li>
    <li><a slot="brand" href="https://piazza.com/" class="nav-link">Piazza Forum</a></li>
    <li slot="right">
      <form class="navbar-form">
        <searchbar :data="searchData" placeholder="Search" :on-hit="searchCallback" menu-align-right></searchbar>
      </form>
    </li>
  </navbar>
</header>