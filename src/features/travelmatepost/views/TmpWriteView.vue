<script setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router'
import QuillEditor from '@/components/common/QuillEditor.vue'
import { useAuthStore } from '@/stores/auth'
import { createTmpPost } from '@/features/travelmatepost/api/travelmatepost.js'

const router = useRouter()
const authStore = useAuthStore()

const title = ref('')
const content = ref('')

const onCancel = () => {
  router.back()
}

const onSubmit = async () => {
  if (!title.value.trim() || !content.value.trim()) {
    alert('제목과 내용을 모두 입력해주세요.')
    return
  }

  try {
    const editor = document.querySelector('.ql-editor');
    const images = editor.querySelectorAll('img');
    const fixedContent = content.value.replace(/\/temp\//g, '/image/');
    const imageUrls = Array.from(images).map(img => img.getAttribute('src'));

    const postData = {
      title: title.value,
      content: fixedContent,
      imageUrls: imageUrls
    }
    const id = await createTmpPost(authStore.accessToken, postData)
    alert('게시글이 등록되었습니다!')
    router.push(`/board/${id}`)
  } catch (e) {
    alert('로그인 후 게시글 등록 가능합니다.', e)
  }
}
</script>

<template>
  <div class="container">
    <div class="notice-title">
      <h2>동행글 작성</h2>
    </div>

    <div class="editor-page">
      <!-- 제목 입력 -->
      <input
        v-model="title"
        type="text"
        placeholder="제목을 입력하세요."
        class="title-input"
      />

      <!-- Quill 에디터 -->
      <QuillEditor v-model="content" />

      <!-- 버튼 -->
      <div class="button-wrapper">
        <button class="cancel-btn" @click="onCancel">취소</button>
        <button class="submit-btn" @click="onSubmit">등록</button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.container {
  width: 100%;
  padding: 40px 100px;
  box-sizing: border-box;
  background-color: #fffef9;
}

.notice-title h2 {
  font-size: 1.25rem;
  margin-bottom: 16px;
  margin-left: 60px;
}

.editor-page {
  width: 100%;
  max-width: 1000px;
  margin: 40px auto;
  background: #fff;
  padding: 30px 40px;
  border: 1px solid #ddd;
  border-radius: 8px;
  box-sizing: border-box;
}

.title-input {
  width: 100%;
  padding: 12px;
  font-size: 1.125rem;
  margin-bottom: 20px;
  border: 1px solid #ccc;
  border-radius: 6px;
}

.button-wrapper {
  text-align: right;
}

.cancel-btn,
.submit-btn {
  padding: 10px 20px;
  font-size: 1rem;
  border: none;
  border-radius: 6px;
  cursor: pointer;
}

.cancel-btn {
  background-color: #E57575;
  color: white;
  margin-right: 10px;
}

.submit-btn {
  background-color: #75A9FF;
  color: white;
}

.cancel-btn:hover,
.submit-btn:hover {
  background-color: #e0e0e0;
}
</style>
