<template>
  <div class="userRegisterView">
    <h2 style="margin-bottom: 16px">用户注册</h2>
    <a-form
      style="max-width: 250px; margin: 0 auto"
      label-align="left"
      auto-label-width
      :model="form"
      @submit="handleSubmit"
    >
      <a-form-item field="userName" label="昵称">
        <a-input v-model="form.userName" placeholder="请输入昵称" />
      </a-form-item>
      <a-form-item field="userAccount" label="账号">
        <a-input v-model="form.userAccount" placeholder="请输入账号" />
      </a-form-item>
      <a-form-item field="userPassword" tooltip="密码不少于 8 位" label="密码">
        <a-input-password
          v-model="form.userPassword"
          placeholder="请输入密码"
        />
      </a-form-item>
      <a-form-item
        field="checkPassword"
        tooltip="密码不少于 8 位"
        label="重复密码"
      >
        <a-input-password
          v-model="form.checkPassword"
          placeholder="请输入密码"
        />
      </a-form-item>
      <a-form-item>
        <a-button
          type="primary"
          html-type="submit"
          style="width: 120px; margin-right: 20px"
          >注册
        </a-button>
        <a-button type="outline" href="/user/login" style="width: 120px"
          >登录
        </a-button>
      </a-form-item>
    </a-form>
  </div>
</template>

<script setup lang="ts">
import { reactive } from "vue";
import message from "@arco-design/web-vue/es/message";
import { useRouter } from "vue-router";
import { useStore } from "vuex";
import { UserControllerService, UserRegisterRequest } from "../../../generated";

/**
 * 表单信息
 */
const form = reactive({
  userName: "",
  checkPassword: "",
  userAccount: "",
  userPassword: "",
} as UserRegisterRequest);

const router = useRouter();
const store = useStore();

/**
 * 提交表单
 * @param data
 */
const handleSubmit = async () => {
  const res = await UserControllerService.userRegisterUsingPost(form);
  // 登陆成功跳转到主页
  if (res.code === 0) {
    await store.dispatch("user/getLoginUser");
    router.push({
      path: "/",
      replace: true,
    });
  } else {
    message.error("注册失败，" + res.message);
  }
};
</script>
