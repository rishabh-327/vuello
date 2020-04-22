<template>
  <div id="board">
    <v-container fluid>
      <!-- Columns  -->
      <v-row class="flex-nowrap" no-gutters>
        <v-col
          v-for="(column, columnIndex) of board.columns"
          :key="columnIndex"
        >
          <BoardColumn :column="column" :columnIndex="columnIndex" />
        </v-col>

        <!-- Add new Column -->
        <v-col>
          <v-card color="secondary" width="352px" class="mr-4" elevation="1">
            <v-card-title class="py-2 pb-0">
              <v-text-field
                clearable
                v-model="newColumnTitle"
                label="Add new column"
                @keydown.enter="addColumn"
                :color="!isDarkTheme ? 'indigo darken-4' : 'blue lighten-5'"
              >
                <template v-slot:prepend>
                  <v-icon
                    :color="!isDarkTheme ? 'indigo darken-4' : 'blue lighten-5'"
                    >mdi-playlist-plus</v-icon
                  >
                </template>
              </v-text-field>
            </v-card-title>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
    <v-dialog v-model="isTaskOpen" width="700" persistent>
      <router-view />
    </v-dialog>
  </div>
</template>
<script>
import { mapState } from 'vuex'
import BoardColumn from '@/components/BoardColumn.vue'

export default {
  name: 'Board',
  data: () => ({
    newColumnTitle: ''
  }),
  computed: {
    ...mapState(['board']),
    isTaskOpen() {
      return this.$route.name === 'task'
    }
  },
  components: {
    BoardColumn
  },
  methods: {
    addColumn() {
      this.$store.commit('ADD_COLUMN', { columnTitle: this.newColumnTitle })
      this.newColumnTitle = ''
    }
  }
}
</script>

<style lang="scss" scoped>
#board {
  overflow: auto;
  height: 100%;
}
</style>
