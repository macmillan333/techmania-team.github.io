<template lang="pug">
  q-page#patterns
    section.q-mx-auto.padding
      .container
        .row
          .col-12.q-mx-auto
            h4.text-center {{ $t('nav.patterns') }}
            q-separator.q-my-md
            br
            q-input(rounded outlined v-model="search.text" :placeholder="$t('patterns.search')" @keydown.enter="applySearch")
              template(v-slot:after)
                q-btn(icon="search" round desnse flat @click="applySearch")
            br
            q-list.search
              q-item.q-py-none
                q-item-section {{ $t('pattern.keysounded') }}
                q-item-section
                  div
                    q-btn(flat size="10px" :label="$t('patterns.all')" :text-color="search.keysounded === -1 ? 'white' : 'grey'" @click="search.keysounded = -1")
                    q-btn(flat size="10px" :label="$t('patterns.yes')" :text-color="search.keysounded === 1 ? 'white' : 'grey'" @click="search.keysounded = 1")
                    q-btn(flat size="10px" :label="$t('patterns.no')" :text-color="search.keysounded === 0 ? 'white' : 'grey'" @click="search.keysounded = 0")
            q-separator.q-my-md
        .row.q-my-md
          .col-12.col-sm-6.col-md-3.q-pa-md.q-my-xs(v-for="(pattern, index) in filteredPatterns" :key="pattern.id")
            PatternCard(:pattern="pattern" :mine="false")
</template>

<script>
import PatternCard from '../components/PatternCard'

export default {
  name: 'PagePatterns',
  meta () {
    return {
      title: 'Patterns | TECHMANIA',
      meta: {
        title: {
          name: 'title',
          content: 'Patterns | TECHMANIA'
        },
        description: {
          name: 'description',
          content: 'Patterns for TECHMANIA.'
        },
        ogType: {
          name: 'og:type',
          content: 'website'
        },
        ogUrl: {
          name: 'og:url',
          content: new URL(this.$route.fullPath, process.env.HOST_URL).toString()
        },
        ogTitle: {
          name: 'og:title',
          content: 'Patterns | TECHMANIA'
        },
        ogDescription: {
          name: 'og:description',
          content: 'Patterns for TECHMANIA.'
        },
        ogImage: {
          name: 'og:image',
          content: 'https://raw.githubusercontent.com/techmania-team/techmania-team.github.io/master/public/assets/Logo_black.png'
        },
        twCard: {
          name: 'twitter:card',
          content: 'summary_large_image'
        },
        twUrl: {
          name: 'twitter:url',
          content: new URL(this.$route.fullPath, process.env.HOST_URL).toString()
        },
        twTitle: {
          name: 'twitter:title',
          content: 'Patterns | TECHMANIA'
        },
        twDescription: {
          name: 'twitter:description',
          content: 'Patterns for TECHMANIA.'
        },
        twImage: {
          name: 'twitter:image',
          content: 'https://raw.githubusercontent.com/techmania-team/techmania-team.github.io/master/public/assets/Logo_black.png'
        }
      }
    }
  },
  components: {
    PatternCard
  },
  data () {
    return {
      patterns: [],
      filter: '',
      error: false,
      submitModel: false,
      search: {
        text: '',
        keysounded: -1
      }
    }
  },
  computed: {
    filteredPatterns () {
      return this.patterns.filter(pattern => {
        const match = pattern.composer.toUpperCase().includes(this.filter.toUpperCase()) || pattern.name.toUpperCase().includes(this.filter.toUpperCase())
        return this.search.keysounded === -1 ? match : this.search.keysounded === 1 ? match && pattern.keysounded : match && !pattern.keysounded
      })
    }
  },
  methods: {
    async fetchPatterns () {
      try {
        const result = await this.$axios.get(new URL('/api/patterns', process.env.HOST_URL))
        if (result.data.success) {
          this.patterns = result.data.result
        } else {
          throw new Error('Error')
        }
      } catch (_) {
        this.error = true
      }
    },
    applySearch () {
      this.filter = this.search.text
    }
  },
  mounted () {
    this.fetchPatterns()
  }
}
</script>
