<template>
  <div>
  </div>
</template>

<script>
  export default {
    layout: 'empty',
    async asyncData({ params,redirect,$cookies }) {
      let affiliateCode = params.id;
      if(!affiliateCode)
        redirect('/');
      $cookies.set('affiliateCode', affiliateCode, {
        path: '/',
        maxAge: 60 * 60 * 24 * 360
      });
      redirect('/');
    }
  }
</script>
