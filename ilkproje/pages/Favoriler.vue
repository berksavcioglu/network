<template>
    <div>
    <app-header/>
    <not-login-favorisayfa/>
    <app-footer/>
</div>
    
</template>

<script>  
import NotLoginFavorisayfa from '../components/FavoriSayfas─▒/NotLoginFavorisayfa.vue';
import AppFooter from '../components/Footer/AppFooter.vue';
import AppHeader from '../components/Header/AppHeader.vue';

export default {
    components: { NotLoginFavorisayfa, AppHeader, AppFooter }
}
</script>