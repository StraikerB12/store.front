<template>
    <div class="header-app">
        <div class="header-app__info"></div>
        <div class="header-app__control">
            <el-button type="primary" @click="redirect">Авторизация VK</el-button> 
            <el-button type="primary" @click="sendCodeAuthVK(variables)" :loading="loading">Токен VK</el-button>
            <el-button color="#626aef00" dark size="large" circle @click="dialogVisible = true">
                <el-icon :size="20"><Tools /></el-icon>
            </el-button>
        </div>

        <el-dialog
            v-model="dialogVisible"
            title="Авторизация"
            width="750"
            class="header-app__dialog"
        >
            <div class="header-app__dialog-content">
                <VkData />
                <span class="header-app__dialog-divider"></span>
                <TgData />
            </div>
        </el-dialog>
    </div>
</template>
<script lang="ts" setup>
    import { ref } from 'vue'
    import { Tools } from '@element-plus/icons-vue'
    import { useMutation } from '@vue/apollo-composable'
    import { getAuthVK } from '@/graphql/querys'
    import { getDataKeys, checkKeysTimes } from '@/store/keysData'
    import VkData from "@/components/VkData.vue"
    import TgData from "@/components/TgData.vue"

    getDataKeys()
    checkKeysTimes()


    const dialogVisible = ref(false)

    function redirect() {
        window.location.replace("https://oauth.vk.com/authorize?client_id=51746034&display=page&redirect_uri=http://des-star.ru&scope=groups&response_type=code&v=5.131");
    }

    const urlParams = new URLSearchParams(window.location.search);
    // const code = urlParams.get('code');


    const variables = ref({
        code: urlParams.get('code'),
        redirectUri: 'http://des-star.ru'
    })

    const { mutate: sendCodeAuthVK, onDone, onError, loading } = useMutation( getAuthVK )


    onDone(result => {
        console.log(result.data)
    })

    onError(error => {
        console.error(error)
    })

</script>
<style lang="scss">
    .header-app {
        &__dialog {
            border-radius: 16px;
            background: #25272a;

            &-content {
                display: flex;
                justify-content: center;

                & > div {
                    width: calc(50% - 21px);
                }
            }

            &-divider {
                display: block;
                width: 1px;
                margin: 0 20px;
                background: $main-border-color;
            }
        }
    }
</style>
<style lang="scss" scoped>
    .header-app {
        display: flex;
        width: 100%;
        padding: 20px 80px;
        background: #25272a;

        &__info {
            flex-grow: 1;
        }

        &__control {
            display: flex;

            & button{
                margin-left: 10PX;
            }
        }
    }
</style>
