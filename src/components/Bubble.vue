<template>
  <!-- @hide="hideLinkMenu"-->
  <!-- { commands, isActive, getMarkAttrs, menu } -->
  <editor-menu-bubble
    v-slot="context"
    :editor="editor"
    :keep-in-bounds="true"
    class="tiptap-vuetify-editor__menububble"
  >
    <!--
    v-show="menu.isActive"
    absolute
    fixed
    // Классы не применяются:
    :content-class="{
      'tiptap-vuetify-editor__menububble-tooltip': true,
      'tiptap-vuetify-editor__menububble-tooltip--is-active': menu.isActive
    }"
    -->
    <!-- getMenuY(context.menu) v-show="context.menu.isActive" -->
    <div>
      <v-card
        :style="getBubbleContentStyle(context)"
        class="tiptap-vuetify-editor__menububble-card"
        dark
      >
        <!--
        <form
          v-if="linkMenuIsActive"
          class="tiptap-vuetify-editor__menububble-form"
          @submit.prevent="setLinkUrl(commands.link, linkUrl)"
        >
          <v-text-field
            ref="linkInput"
            v-model="linkUrl"
            placeholder="Link"
            hide-details
            solo
            @keydown.esc="hideLinkMenu"
          />

          <v-btn
            color="success"
            type="submit"
            icon
          >
            <v-icon>
              {{ getIconByKey('save') }}
            </v-icon>
          </v-btn>

          <v-btn
            color="error"
            icon
            @click="setLinkUrl(commands.link, null)"
          >
            <v-icon>
              {{ getIconByKey('cancel') }}
            </v-icon>
          </v-btn>
        </form>

        <v-btn
          v-else
          :class="{ 'v-btn--active': isActive.link() }"
          color="primary"
          small
          @click="showLinkMenu(getMarkAttrs('link'))"
        >
          <v-icon left>
            {{ getIconByKey(
              isActive.link() ? 'linkUpdate' : 'linkAdd'
            ) }}
          </v-icon>

          {{ isActive.link() ? $i18n.getMsg('extensions.Link.bubble.updateLink') : $i18n.getMsg('extensions.Link.bubble.addLink') }}
        </v-btn>
        -->

        <actions-render
          :actions="actions"
          :context="context"
          :editor="editor"
          :dark="true"
        />
      </v-card>
    </div>
  </editor-menu-bubble>
</template>

<script lang="ts">
import { Component, Prop } from 'vue-property-decorator'
import { mixins } from 'vue-class-component'
import { Editor, EditorMenuBubble } from 'tiptap'
// import { icons } from '~/extensions/nativeExtensions/link/Link'
import I18nMixin from '~/mixins/I18nMixin'
import ExtensionActionInterface from '~/extensions/actions/ExtensionActionInterface'
import ActionsRender from '~/components/ActionsRender.vue'
import { VCard } from 'vuetify/lib'

@Component({
  components: {
    ActionsRender,
    EditorMenuBubble,
    VCard
  }
})
export default class Menu extends mixins(I18nMixin) {
  @Prop({ type: Object, required: true })
  readonly editor!: Editor

  @Prop({
    type: Array,
    default: () => []
  })
  readonly actions: ExtensionActionInterface[]

  // linkUrl: null | string = null
  // linkMenuIsActive: null | boolean = false

  getMenuY (menu) {
    // высота всей страницы - высота окна - сколько страницу прокрученно от верха
    const diff = document.documentElement.scrollHeight - window.innerHeight - window.scrollY
    // bottom позиция относитель низа окна
    const bottomRelatedToWindow = menu.bottom - diff

    // top позиция
    return window.innerHeight - bottomRelatedToWindow + 15 // + 15 из-за того что bottom, если top, то не нужно
  }

  getBubbleContentStyle (context) {
    return {
      display: context.menu.isActive ? 'block' : 'none',
      // иначе не видно
      zIndex: 1,
      position: 'absolute',
      left: context.menu.left + 'px',
      bottom: context.menu.bottom + 'px'
    }
  }

  // getIconByKey (key) {
  //   return icons[key][this.$tiptapVuetify.iconsGroup]
  // }

  // showLinkMenu (attrs) {
  //   this.linkUrl = attrs.href
  //   this.linkMenuIsActive = true
  //   this.$nextTick(() => {
  //     // @ts-ignore
  //     this.$refs.linkInput.focus()
  //   })
  // }

  // hideLinkMenu () {
  //   this.linkUrl = null
  //   this.linkMenuIsActive = false
  // }

  // setLinkUrl (command, url) {
  //   command({ href: url })
  //   this.hideLinkMenu()
  //   this.editor.focus()
  // }
}
</script>

<style lang="stylus">
  .tiptap-vuetify-editor
    &__menububble-card
      opacity: 0.9 !important
      padding: 5px
</style>
