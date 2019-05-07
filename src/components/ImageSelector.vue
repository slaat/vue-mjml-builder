<template>
  <div class="text-xs-center">
    <v-img
      :src="image"
      style="cursor: pointer"
      @click="pickFile"
      :loading="loading"
    ></v-img>
    <v-text-field
      :class="{ 'hidden': !showText }"
      v-model="imageName"
      :rules="rules"
      :label="fieldName"
      prepend-icon="attach_file"
      @click="pickFile"
    />
    <input
      ref="image"
      type="file"
      style="display: none"
      accept="image/*"
      @change="onFilePicked"
    >
  </div>
</template>

<script lang="ts">
  import {Component, Vue, Prop, Watch} from 'vue-property-decorator';
  import {Storage} from 'aws-amplify';
  // import {mapActions} from 'vuex';

  // import awsExports from '@/aws-exports.js';

  @Component
  export default class ImageSelector extends Vue {
    public $refs!: {
      image: any,
    };

    private loading: boolean = false;
    private imageName: string = '';
    private imageFile: string = '';
    private image: string | object = 'placeholder.png';

    @Prop({type: Number, default: 400}) private width!: any;
    @Prop({type: String, default: 'Logo'}) private fieldName!: any;
    @Prop({type: Boolean, default: false}) private showText!: boolean;
    @Prop({type: File, default: null}) private value!: any;
    @Prop({type: String, default: ''}) private url!: any;
    @Prop({type: Array, default: () => [(v) => !!v || 'Dies ist ein Pflichtfeld']}) private rules!: any;

    private mounted() {
      // tslint:disable-next-line
      // console.log(awsExports.aws_user_files_s3_bucket);
    }

    private created() {
      if (this.url) {
        this.image = this.url;
      }
    }

    @Watch('url')
    private updatedUrl() {
      if (this.url) {
        this.image = this.url;
      }
    }

    @Watch('value')
    private updatedValue() {
      this.loading = true;

      if (!this.value) {
        this.loading = false;
        this.imageName = '';
        this.imageFile = '';
        this.image = 'placeholder.png';
        this.$refs.image.value = '';
      } else {
        this.imageName = this.value.name;
        const fr = new FileReader();
        fr.readAsDataURL(this.value);
        fr.addEventListener('load', (res: any) => {
          this.image = res.target.result;
          this.loading = false;
        });
      }
    }

    private pickFile() {
      this.$refs.image.click();
    }

    private onFilePicked(e: any) {
      this.loading = true;

      const files = e.target.files;
      if (files[0] !== undefined) {
        this.imageName = files[0].name;
        if (this.imageName.lastIndexOf('.') <= 0) {
          this.loading = false;
          return;
        }
        const fr = new FileReader();
        fr.readAsDataURL(files[0]);
        fr.addEventListener('load', (res: any) => {
          this.image = res.target.result;

          this.$emit('input', files[0]);
          this.loading = false;
        });
      }
    }

    private doUpload() {
      // return this.uploadImage(this.imageFile);
    }
  }
</script>

<style scoped>
  .hidden {
    display: none;
  }
</style>
