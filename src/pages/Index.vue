<template>
  <div>
    <q-card class="q-ma-md">
      <div class="text-h6 q-ml-md">Validação dinamica v-if</div>
      <q-separator/>

      <div class="row">
        <q-input
          class="q-ma-md"
          v-model="params.text"
          label="Nome"
          error-message="Nome é Obrigatório"
          :error="this.$v.params.text.$error"
        />

        <q-toggle v-model="ecpf" label="Sobrenome"/>

        <div v-if="ecpf">
          <q-input
            class="q-ma-md"
            v-model="params.text2"
            label="Sobrenome"
            error-message="Informe o sobrenome"
            :error="this.$v.params.text2.$error"
          />
        </div>
        <div class="col q-ma-md">
          <q-btn class="float-right q-mt-md" color="secondary" label="Enviar" @click="enviar()"/>
        </div>
      </div>
    </q-card>

    <q-card class="q-ma-md">
      <div class="text-h6 q-ml-md">Validação dinamica v-if</div>
      <q-separator/>

      <div v-for="(line, index) in lines" v-bind:key="index" class="row">
        <div class="col-lg-6">
          <div class="row">
            <div class="col-10">
              <q-field>
                <q-input
                  label="Phone"
                  v-model="line.number"
                  error-message="Informe o sobrenome"
                  :error="this.$v.lines.number.$error"/>
              </q-field>
            </div>
          </div>
        </div>

        <div class="col-lg-2">
          <div class="block float-right">
            <q-btn @click="removeLine(index)" icon="delete" round/>
            <q-btn v-if="index + 1 === lines.length" @click="addLine" icon="add" round/>
          </div>
        </div>
      </div>
    </q-card>
  </div>
</template>

<style>
</style>

<script>
import { required } from 'vuelidate/lib/validators'
export default {
  validations () {
    if (!this.ecpf) {
      return {
        params: {
          text: { required }
        },
        lines: {
          $each: {
            number: {
              required
            }
          }
        }
      }
    } else {
      return {
        params: {
          text: { required },
          text2: { required }
        },
        lines: {
          $each: {
            number: {
              required
            }
          }
        }
      }
    }
  },
  watch: {
    lines () {
      this.blockRemoval = this.lines.length <= 1
    }
  },
  data () {
    return {
      params: {
        text: '',
        text2: ''
      },
      ecpf: false,
      lines: [{ number: '' }],
      blockRemoval: true
    }
  },
  methods: {
    enviar () {
      this.$v.$touch()
      if (!this.$v.$invalid) {
        console.log('REQUEST IS HERE')
      }
    },
    addLine () {
      let checkEmptyLines = this.lines.filter(line => line.number === null)
      if (checkEmptyLines.length >= 1 && this.lines.length > 0) return
      this.lines.push({
        number: null
      })
    },
    removeLine (lineId) {
      if (!this.blockRemoval) this.lines.splice(lineId, 1)
    }
  },
  mounted () {
    this.addLine()
  }
}
</script>
