<template>
  <div>
    <b-container id="write-section" fluid>
      <h2 style="margin-bottom: 40px">도서 {{ inputModeText }}</h2>

      <b-form>
        <b-form-group
          label="ISBN 번호"
          label-for="storage-isbn"
          :description="`${inputModeText}할 도서의 ISBN 번호를 입력해주세요.`"
          :state="Boolean(storage.isbn)"
        >
          <b-form-input
            id="storage-isbn"
            v-model="storage.isbn"
            type="text"
            placeholder="ISBN 번호를 입력해주세요."
            :state="Boolean(storage.isbn)"
            :disabled="inputMode === 'update'"
          ></b-form-input>
        </b-form-group>

        <b-form-group
          label="중고도서 가격"
          label-for="storage-price"
          :description="`${inputModeText}할 도서의 가격을 입력해주세요.`"
          :state="Boolean(storage.price)"
        >
          <b-form-input
            id="storage-price"
            v-model="storage.price"
            type="text"
            placeholder="도서 가격을 입력해주세요."
            :state="Boolean(storage.price)"
          ></b-form-input>
        </b-form-group>

        <b-form-group
          label="중고도서 위치"
          label-for="storage-location"
          :description="`${inputModeText}할 도서의 위치를 입력해주세요.`"
          :state="Boolean(storage.location)"
        >
          <b-form-input
            id="storage-location"
            v-model="storage.location"
            type="text"
            placeholder="도서 위치를 입력해주세요."
            :state="Boolean(storage.location)"
          ></b-form-input>
        </b-form-group>

        <b-form-group
          label="도서 리뷰"
          label-for="storage-review"
          :description="`${inputModeText}할 도서의 리뷰를 입력해주세요.`"
          :state="Boolean(storage.review)"
        >
          <b-form-textarea
            id="storage-review"
            v-model="storage.review"
            type="text"
            :placeholder="storage.review == null ? '리뷰가 존재하지 않습니다.' : '리뷰를 입력해주세요.'"
            rows="3"
            max-rows="6"
            no-resize
          ></b-form-textarea>
        </b-form-group>

        <div style="margin-top: 40px">
          <b-button variant="primary" @click="onSubmit">도서 {{ inputModeText }}</b-button>
          <b-button variant="danger" @click="$router.push('/home')">{{ inputModeText }} 취소</b-button>
        </div>
      </b-form>
    </b-container>
  </div>
</template>

<script>
export default {
  data() {
    return {
      storage: {
        isbn: null,
        price: null,
        location: null,
        review: null
      }
    }
  },
  computed: {
    infoData() {
      return this.$store.getters.Storage
    },
    inputMode() {
      return this.$store.getters.StorageInputMode
    },
    inputModeText() {
      return this.inputMode == 'insert' ? '등록' : '수정'
    }
  },
  watch: {
    // infoData는 regi 화면에 들어온 이후에 감지됩니다.
    infoData(value) {
      this.storage = { ...value }
    }
  },
  created() {
    // created()는 regi 화면이 최초 작성될 때 감지됩니다.
    this.storage = { ...this.infoData }
  },
  methods: {
    onSubmit() {
      if (this.storage.isbn) {
        // 1. 등록인 경우
        if (this.inputMode === 'insert') {
          this.$store.dispatch('actStorageInsert', this.storage) // 중고도서 입력 실행
        }

        // 2. 수정인 경우
        if (this.inputMode === 'update') {
          this.$store.dispatch('actStorageUpdate', this.storage) // 중고도서 수정 실행
        }

        this.$router.push('/home')
      }
    }
  }
}
</script>

<style lang="scss" scoped></style>
