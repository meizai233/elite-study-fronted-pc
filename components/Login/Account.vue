<script lang="ts" setup>
import { message } from "ant-design-vue";
import { login } from "~/api/account";
let { changeToForget, loginModel } = $(useModel());
let { switchLoginState } = $(useUser());

// 初始值
const currentInfo = reactive({
  phone: "",
  password: "",
});

// 校验规则
const rules = {
  phone: [{ required: true, trigger: "blur", message: "请输入手机号!" }],
  password: [{ required: true, trigger: "blur", message: "请输入密码!" }],
};

// 表单提交
const onFinish = async () => {
  // 登录接口
  const res = await login({ phone: currentInfo.phone, password: currentInfo.password });
  if (res.code === 0) {
    // 在这里 把token放在pinia 持久化 cookie中
    switchLoginState(res.data);
    loginModel = false;
    message.success("登录成功");
  }
};
</script>

<template>
  <div>
    <a-form autocomplete="off" ref="formRef" :model="currentInfo" @finish="onFinish">
      <a-form-item name="phone" :rules="rules.phone">
        <a-input placeholder="请输入手机号" v-model:value="currentInfo.phone" />
      </a-form-item>
      <a-form-item name="password" :rules="rules.password">
        <a-input-password type="password" placeholder="请输入密码" autoComplete="false" v-model:value="currentInfo.password" />
      </a-form-item>
      <div flexb>
        <a-form-item>
          <div flex mt-2px text-10px>
            <p>登录即同意优研平台</p>
            <a ml-4px color="#169bd5" target="__blank">《隐私政策》</a>
          </div>
        </a-form-item>

        <a-form-item>
          <span cursor-pointer text-center text-12px color="#555555" @click="changeToForget">忘记密码</span>
        </a-form-item>
      </div>
      <a-form-item>
        <button type="submit" bg="#4d555d" class="rounded-full btn center-text-36 w-300px text-white">立即登录</button>
      </a-form-item>
    </a-form>
    <Bottom type="login" />
  </div>
</template>
