<template>
  <div class="card">
    <div class="card-body">
      <h5 class="card-title">
        <slot name="headerCard"></slot>
      </h5>
      <div class="card-text">
        <slot name="contentCard"></slot>
      </div>
    </div>
  </div>
</template>
