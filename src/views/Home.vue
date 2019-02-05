<template>
  <div>
    <div v-if="!played && !waiting" class="cta">
      <button @click.prevent.stop="buildList">
        Sortear cartões
      </button>
    </div>

    <div v-if="waiting" class="waiting">
      A sortear cartões ...
    </div>
    <template v-else>
      <ul v-if="shuffledUsers">
        <li v-for="(user, i) in shuffledUsers" :key="i">
          <div class="number">{{ i + 1 }}</div>

          <img :src="buildImagePath(i + 1)" alt="">

          <div>
            <div class="name">{{ user }}</div>
          </div>
        </li>
      </ul>
    </template>
  </div>
</template>

<script>
export default {
  data: () => ({
    classes: {
      fm2: ['Maria W.', 'Maria F.', 'Matilde', 'Hugo', 'Francisco', 'Afonso', 'Joana'],
      fm3: ['Tatiana', 'Patrícia', 'Rafaela', 'Leonor', 'Jéssica', 'Gustavo', 'Davide', 'Martim']
    },
    activeClass: 'fm3',
    shuffledUsers: null,
    waiting: false,
    played: false
  }),
  computed: {
    cardsPath () {
      return '/' + this.activeClass + '/'
    }
  },
  methods: {
    buildImagePath (key) {
      return require('../assets/' + this.activeClass + '/' + key + '.png')
    },
    buildList () {
      this.waiting = true

      setTimeout(() => {
        this.shuffle()
      }, 2000)
    },
    shuffle () {
      let users = [...this.classes[this.activeClass]]

      for (let i = users.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [users[i], users[j]] = [users[j], users[i]];
      }

      this.shuffledUsers = users
      this.waiting = false
      this.played = true
    }
  }
}
</script>

<style lang="scss" scoped>
button {
  font-size: 2.2rem;
  padding: 1.5rem 2rem;
  display: inline-block;
  text-align: center;
  border: 1px solid transparent;
  border-radius: 0.25rem;
  transition: color 0.15s ease-in-out, background-color 0.15s ease-in-out, border-color 0.15s ease-in-out, box-shadow 0.15s ease-in-out;
  color: #fff;
  background: #61a2e4;
  font-weight: 400;
  cursor: pointer;
}

ul {
  padding: 2rem 0;

  > li {
    position: relative;
    display: inline-block;
    width: 40vw;
    padding: 1.5rem 0.5rem 0;
    margin: 1rem 1rem 2rem;
    border: 1px solid #61a2e4;

    .number {
      position: absolute;
      top: -1.4rem;
      left: 45%;
      border-radius: 50%;
      border: 1px solid #61a2e4;
      color: #fff;
      width: 2.5rem;
      height: 2.5rem;
      line-height: 2.5rem;
      text-align: center;
      font-size: 1.2rem;
      font-weight: bold;
      background: #61a2e4;
    }

    .name {
      display: inline-block;
      color: #444;
      font-weight: bold;
      border: 2px dashed #999;
      padding: 0.5rem 1.75rem;
      margin: 1rem 0;
    }

    img {
      max-width: 100%;
      max-height: 90px;
    }
  }
}

.cta {
  position: fixed;
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.waiting {
  font-size: 2.2rem;
  text-align: center;
  font-weight: bold;
  color: #61a2e4;
}
</style>
