<template>
  <div>
    <p>Click on the links below for more info:</p>
    <nav class="navbar navbar-toggleable-md navbar-light">
      <ul class="nav">
        <router-link :to="{name : 'historyLink'}" tag="li">
          <a class="nav-link">Our history</a>
        </router-link>
        <router-link :to="{name : 'contactLink'}" tag="li">
          <a class="nav-link">Contact us</a>
        </router-link>
        <router-link :to="{name : 'orderingGuideLink'}" tag="li">
          <a class="nav-link">Ordering guide</a>
        </router-link>
        <router-link :to="{name : 'deliveryLink'}" tag="li">
          <a class="nav-link">Delivery</a>
        </router-link>
      </ul>
    </nav>
    <router-view></router-view>
  </div>
</template>


<script>
export default {
  data() {
    return {};
  }
};
</script>