<template>
  <div>
    <progress-head :amount="5" :passed="5"></progress-head>
    <h1 class="text-ls font-bold">
      ยืนยันการเลือกสภานักศึกษา <br />
      {{ faculty }}
    </h1>
    <div id="body" class="mx-10">
      <div
        class="w-full bg-white rounded-xl px-2 flex flex-col space-y-2 border-2 border-black pt-5 min-h-72 pb-5"
      >
        <confirm-student-card
          v-for="(student, index) in getStudents"
          :key="index"
          :student="student"
        ></confirm-student-card>
      </div>
      <h1 class="text-md mt-5">
        เมื่อเลือก <span class="font-bold text-lg">“เสร็จสิ้น”</span><br />
        จะไม่สามารถแก้ไขได้อีก <br />คุณจะยืนยันการเลือกหรือไม่
      </h1>
      <div class="flex justify-between mt-3">
        <button class="w-28 bg-gray-400 rounded-lg py-1.5 px-3" @click="edit">
          แก้ไข
        </button>
        <button
          class="w-28 bg-white rounded-lg py-1.5 px-3 font-extrabold"
          @click="submit"
        >
          เสร็จสิ้น
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import ProgressHead from "@/components/ProgressHead.vue";
import ConfirmStudentCard from "@/components/ConfirmStudentCard.vue";
import axios from "axios";
export default {
  name: "ConfirmSelectSecond",
  data() {
    return {
      faculty: this.$store.getters.getUser.faculty,
    };
  },
  methods: {
    edit() {
      this.$router.push("SelectSecond");
    },
    submit() {
      let studentCouncil = [];
      let parties = [];
      this.$store.getters.getConcil.forEach((student) => {
        let pushingStudent = {};
        pushingStudent.id = student.id;
        pushingStudent.choice = student.selectState;
        studentCouncil.push(pushingStudent);
      });
      this.$store.getters.getParties.forEach((party) => {
        let pushingParty = {};
        pushingParty.id = party.id;
        pushingParty.choice = party.isAccept;
        parties.push(pushingParty);
      });

      axios({
        method: "POST",
        url: this.$store.getters.getAPIPath + "/api/vote/",
        headers: {
          Authorization: this.$store.getters.getToken,
        },
        data: {
          council: studentCouncil,
          party: parties,
        },
      })
        .then((result) => {
          console.log(result);
          this.$router.push("Finish");
        })
        .catch((error) => {
          const x = JSON.parse(error.response.data.message);
          alert(x.th);
          setTimeout(() => this.$router.push("Login"), 3000);
        });
    },
  },
  computed: {
    getStudents() {
      return this.$store.getters.getConcil;
    },
  },
  components: {
    ProgressHead,
    ConfirmStudentCard,
  },
};
</script>

<style></style>
