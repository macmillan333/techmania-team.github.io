<template lang="pug">
  q-card.full-height.card-pattern
    q-img(:src="`http://i3.ytimg.com/vi/${pattern.previews[0].ytid}/hqdefault.jpg`" @click="$router.push('/patterns/'+pattern._id)" @error="imgerror" :ratio="16/9")
      .absolute-bottom
        .text-h6 {{ pattern.name }}
        .text-subtitle2  {{ pattern.composer }}
    q-card-section
      q-btn.btn-dl.absolute(v-if="!mine" fab icon="download" color="tech" text-color="black" @click="openLink(pattern.link)")
      q-btn.btn-dl.absolute(v-if="mine" fab icon="edit" color="tech" text-color="black" @click="$emit('edit')")
      .row.no-wrap.items-center
      q-list
        q-item
          q-item-section
            p
              span(v-if="!mine") {{ $t('pattern.author') }} {{ pattern.submitter.name }}
              br(v-if="!mine")
              span {{ $t('pattern.submitted') }} {{ formattedTime.relative }}
                q-tooltip(anchor="top middle" self="bottom middle" content-style="background: #000")
                  | {{ formattedTime.text }}
              br
              span {{ $t('pattern.updated') }} {{ formattedUpdateTime.relative }}
                q-tooltip(anchor="top middle" self="bottom middle" content-style="background: #000")
                  | {{ formattedUpdateTime.text }}
        q-item
          q-item-section(:class="[{'text-red': !pattern.keysounded, 'text-positive': pattern.keysounded}]")
            div
              q-icon(:name="!pattern.keysounded ? 'close' : 'check'")
              | &nbsp;{{ $t('pattern.keysounded') }}
        q-item
          q-item-section
            div.q-gutter-sm
              q-icon(
                v-for="(difficulty, index) in pattern.difficulties"
                :key="'D'+index"
                size="sm"
                :name="getControlIcon(difficulty.control, difficulty.level)"
                :class="getLevelColor(difficulty.level)"
              )
                q-tooltip(anchor="top middle" self="bottom middle" content-style="background: #000")
                  | {{ $t('pattern.'+controls[difficulty.control]) }} - {{ difficulty.name }}
                  br
                  span.text-bold(:class="getLevelColor(difficulty.level)") Lv.{{ difficulty.level }}
</template>

<script>
export default {
  name: 'PatternCard',
  props: {
    pattern: Object,
    mine: Boolean
  },
  methods: {
    imgerror () {
      console.log('a')
    }
  },
  computed: {
    formattedTime () {
      return {
        relative: this.$date(this.pattern.submitDate).locale(this.user.locale).fromNow(),
        text: new Date(this.pattern.submitDate).toLocaleString(this.user.locale)
      }
    },
    formattedUpdateTime () {
      return {
        relative: this.$date(this.pattern.updateDate).locale(this.user.locale).fromNow(),
        text: new Date(this.pattern.updateDate).toLocaleString(this.user.locale)
      }
    }
  }
}
</script>
