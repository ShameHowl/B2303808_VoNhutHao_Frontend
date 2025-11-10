<template>
  <div class="page">
    <h4>Thêm Liên hệ</h4>
    <ContactForm :contact="contact" @submit:contact="addContact" />
    <p>{{ message }}</p>
  </div>
</template>

<script>
import ContactForm from "@/components/ContactForm.vue";
import ContactService from "@/services/contact.service";

export default {
  components: {
    ContactForm,
  },
  data() {
    return {
      contact: {
        name: "",
        email: "",
        address: "",
        phone: "",
        favorite: false,
      },
      message: "",
    };
  },
  methods: {
    async addContact(data) {
      try {
        console.log("Dữ liệu gửi đi:", data);
        const result = await ContactService.create(data);
        console.log("Kết quả trả về:", result);
        this.message = "Liên hệ được thêm thành công.";
        alert("Liên hệ được thêm thành công.");
        this.$router.push({ name: "contactbook" });
      } catch (error) {
        console.error("Chi tiết lỗi:", error);
        console.error("Response lỗi:", error.response);
        if (error.response) {
          this.message = `Lỗi ${error.response.status}: ${error.response.data.message || error.response.statusText}`;
        } else if (error.request) {
          this.message =
            "Không thể kết nối đến server. Vui lòng kiểm tra backend đã chạy chưa.";
        } else {
          this.message = `Có lỗi xảy ra: ${error.message}`;
        }
        alert(this.message);
      }
    },
  },
  created() {
    this.message = "";
  },
};
</script>

<style scoped>
.page {
  padding: 20px;
}

h4 {
  margin-bottom: 20px;
  color: #333;
}

p {
  margin-top: 15px;
  font-weight: 500;
}
</style>
