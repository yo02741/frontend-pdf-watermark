<script setup>
import { ref, reactive } from "vue";
import html2canvas from 'html2canvas';
import jsPDF from 'jspdf';

const refMyForm = ref(null);
const form = reactive({
  name: "",
  region: "",
  date1: "",
  date2: "",
  delivery: false,
  type: [],
  resource: "",
  desc: "",
});

const onExportPdf = async () => {
  const formPadding = 10;

  const w = refMyForm.value.scrollWidth;
  const h = refMyForm.value.scrollHeight;

  const canvas = await html2canvas(refMyForm.value, {
    scale: 5,
    useCORS: true,
    allowTaint: true,
    logging: true,
    width: w + formPadding * 2,
    height: h + formPadding * 2,
  });

  const imgData = canvas.toDataURL("image/png");
  const pdf = new jsPDF("p", "mm", "a4");
  const scaleFactor = (pdf.internal.pageSize.width - formPadding * 2) / canvas.width;

  pdf.addImage(imgData, "PNG", formPadding, formPadding, canvas.width * scaleFactor, canvas.height * scaleFactor);
  pdf.save("my-form.pdf");
};

const onExportPdfWithWatermark = () => {
  console.log("onExportPdfWithWatermark");
};
</script>

<template>
  <div ref="refMyForm" class="my-form">
    <el-form :model="form" label-width="120px">
      <el-form-item label="">
        <h1>Hello World</h1>
      </el-form-item>
      <el-form-item label="Activity name">
        <el-input v-model="form.name" />
      </el-form-item>
      <el-form-item label="Activity zone">
        <el-select v-model="form.region" placeholder="please select your zone">
          <el-option label="Zone one" value="shanghai" />
          <el-option label="Zone two" value="beijing" />
        </el-select>
      </el-form-item>
      <el-form-item label="Activity time">
        <el-col :span="11">
          <el-date-picker
            v-model="form.date1"
            type="date"
            placeholder="Pick a date"
            style="width: 100%"
          />
        </el-col>
        <el-col :span="2" class="text-center">
          <span class="text-gray-500">-</span>
        </el-col>
        <el-col :span="11">
          <el-time-picker
            v-model="form.date2"
            placeholder="Pick a time"
            style="width: 100%"
          />
        </el-col>
      </el-form-item>
      <el-form-item label="Instant delivery">
        <el-switch v-model="form.delivery" />
      </el-form-item>
      <el-form-item label="Activity type">
        <el-checkbox-group v-model="form.type">
          <el-checkbox label="Online activities" name="type" />
          <el-checkbox label="Promotion activities" name="type" />
          <el-checkbox label="Offline activities" name="type" />
          <el-checkbox label="Simple brand exposure" name="type" />
        </el-checkbox-group>
      </el-form-item>
      <el-form-item label="Resources">
        <el-radio-group v-model="form.resource">
          <el-radio label="Sponsor" />
          <el-radio label="Venue" />
        </el-radio-group>
      </el-form-item>
      <el-form-item label="Activity form">
        <el-input v-model="form.desc" type="textarea" />
      </el-form-item>
    </el-form>
  </div>
  <div class="button-group">
    <el-button type="primary" @click="onExportPdf">Export PDF</el-button>
    <el-button type="warning" @click="onExportPdfWithWatermark">Export PDF with Watermark</el-button>
  </div>
</template>

<style lang="scss" scoped>
.my-form {
  width: 100%;
  padding: 20px 30px;
  border: 1px solid #eee;
  border-radius: 5px;
  background-color: honeydew;
}

.button-group {
  display: flex;
  justify-content: center;
  margin-top: 20px;
}
</style>