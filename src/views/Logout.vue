<template>
  <div id="my-10">
    <div id="header">
      <progress-head
        :amount="5"
        :passed="5"
        v-if="!isPrivilegesUsed"
      ></progress-head>
      <progress-head amount="5" passed="5" v-else></progress-head>
      <h1 class="text-2xl font-bold">การลงคะแนนเสียงเสร็จสมบูรณ์</h1>
    </div>
    <div class="mx-10">
      <user-card :student="getUser"></user-card>
    </div>
    <p>
      คุณได้ใช้สิทธิ์ของคุณแล้ว
      ระบบได้ทำการบันทึกข้อมูลและออกจากระบบให้คุณเรียบร้อยแล้ว
    </p>
  </div>
</template>

<script></script>

<style></style>
