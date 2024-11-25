<template>
  <div v-if="editor" class="m-4 flex flex-col rounded-lg bg-slate-50 shadow-xl">
    <div
      class="flex w-[600px] items-center justify-between rounded-t-lg border-b-2 p-3"
    >
      <div>
        <Icon :active="editor.isActive('bold')">
          <Bold @click="editor.chain().focus().toggleBold().run()" />
        </Icon>

        <Icon :active="editor.isActive('italic')">
          <Italic @click="editor.chain().focus().toggleItalic().run()" />
        </Icon>

        <Icon :active="editor.isActive('strike')">
          <Strikethrough @click="editor.chain().focus().toggleStrike().run()" />
        </Icon>

        <Icon :active="editor.isActive('heading', { level: 1 })">
          <Heading1
            @click="editor.chain().focus().toggleHeading({ level: 1 }).run()"
          />
        </Icon>

        <Icon :active="editor.isActive('heading', { level: 2 })">
          <Heading2
            @click="editor.chain().focus().toggleHeading({ level: 2 }).run()"
          />
        </Icon>

        <Icon :active="editor.isActive('heading', { level: 3 })">
          <Heading3
            @click="editor.chain().focus().toggleHeading({ level: 3 }).run()"
          />
        </Icon>

        <Icon :active="editor.isActive({ textAlign: 'left' })">
          <AlignLeft
            @click="editor.chain().focus().setTextAlign('left').run()"
          />
        </Icon>

        <Icon :active="editor.isActive({ textAlign: 'center' })">
          <AlignCenter
            @click="editor.chain().focus().setTextAlign('center').run()"
          />
        </Icon>

        <Icon :active="editor.isActive({ textAlign: 'right' })">
          <AlignRight
            @click="editor.chain().focus().setTextAlign('right').run()"
          />
        </Icon>
      </div>

      <div>
        <Icon class="ml-8">
          <Undo2 @click="editor.commands.undo()" />
        </Icon>

        <Icon>
          <Redo2 @click="editor.commands.redo()" />
        </Icon>
      </div>
    </div>

    <EditorContent
      class="prose prose-lg h-[450px] w-[600px] overflow-auto p-4"
      :editor="editor"
    />

    <div class="flex flex-col items-end justify-center">
      <button
        @click="saveContent"
        class="button font-c-white m-2 rounded-lg bg-sky-700 px-4 py-2 text-white transition-all duration-300 hover:bg-sky-600"
      >
        Save Content
      </button>
    </div>
  </div>
</template>

<script setup lang="ts">
import { onBeforeUnmount } from "vue";
import { EditorContent, useEditor } from "@tiptap/vue-3";
import StarterKit from "@tiptap/starter-kit";
import TextAlign from "@tiptap/extension-text-align";
import {
  Bold,
  Italic,
  Strikethrough,
  Heading1,
  Heading2,
  Heading3,
  AlignLeft,
  AlignRight,
  AlignCenter,
  Undo2,
  Redo2,
} from "lucide-vue-next";

const htmlContent = defineModel("htmlContent");

const initialContent = `
  <h1>Heading 1</h1>
  <h2>Heading 2</h2>
  <h3>Heading 3</h3>
  <h4>Heading 4</h4>
  <h5>Heading 5</h5>
  <h6>Heading 6</h6>`;

const editor = useEditor({
  editorProps: {
    attributes: {
      class: "outline-none",
    },
  },
  content: initialContent,
  extensions: [
    StarterKit,
    TextAlign.configure({
      types: ["heading", "paragraph"],
    }),
  ],
});

onBeforeUnmount(() => {
  editor.value?.destroy();
});

const saveContent = () => {
  if (editor.value) {
    htmlContent.value = editor.value.getHTML();
  }
};
</script>
