<template>
  <v-card>
    <v-data-table
      :headers="headers"
      :items="annotations"
      @input="update"
      item-key="id"
      hide-default-header
      hide-default-footer
      disable-pagination
      class="elevation-1"
    >
      <template v-slot:top>
        <v-text-field
          v-model="newText"
          @keyup.enter="create"
          prepend-inner-icon="mdi-pencil"
          label="New text"
          autofocus
          single-line
          hide-details
          filled
        />
      </template>
      <template v-slot:item.text="{ item }">
        <v-edit-dialog>
          <span class="title" style="font-weight:400">
            {{ item.text }}
          </span>
          <template v-slot:input>
            <v-textarea
              :value="item.text"
              @change="update(item.id, $event)"
              label="Edit"
              autofocus
            />
          </template>
        </v-edit-dialog>
      </template>
      <template v-slot:item.action="{ item }">
        <v-icon
          @click="deleteAnnotation(item.id)"
          small
        >
          delete
        </v-icon>
      </template>
    </v-data-table>
  </v-card>
</template>

<script>
export default {
  props: {
    annotations: {
      type: Array,
      default: () => ([]),
      required: true
    },
    deleteAnnotation: {
      type: Function,
      default: () => ([]),
      required: true
    },
    updateAnnotation: {
      type: Function,
      default: () => ([]),
      required: true
    },
    createAnnotation: {
      type: Function,
      default: () => ([]),
      required: true
    }
  },

  data() {
    return {
      newText: '',
      headers: [
        {
          text: 'Text',
          align: 'left',
          value: 'text'
        },
        {
          text: 'Actions',
          align: 'right',
          value: 'action'
        }
      ]
    }
  },

  methods: {
    update(annotationId, text) {
      if (text.length > 0) {
        this.updateAnnotation(annotationId, text)
      } else {
        this.deleteAnnotation(annotationId)
      }
    },
    create() {
      if (this.newText.length > 0) {
        this.createAnnotation(this.newText)
        this.newText = ''
      }
    }
  }
}
</script>
