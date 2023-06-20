<script setup>
import { useRoute, useRouter } from 'vue-router'
import { FormKit } from '@formkit/vue'
import RouterLink from '../components/ui/RouterLink.vue'
import Heading from '../components/ui/Heading.vue'
import { onMounted, reactive } from 'vue'
import ClienteService from '../services/ClienteService'
const router = useRouter()
const route = useRoute()
const { id } = route.params
const formData = reactive({})
onMounted(() => {
  ClienteService.obtenerCliente(id)
    .then(({ data }) => {
      Object.assign(formData, data)
    })
    .catch((error) => console.log(error))
})
defineProps({
  titulo: {
    type: String
  }
})
const handleSubmit = (data) => {
    ClienteService.actualizarCliente(id, data)
        .then(() => router.push({ name: 'listado-clientes' }))
        .catch((error) => console.log(error))
}
</script>

<template>
  <div>
    <div class="flex justify-end">
      <RouterLink to="listado-clientes"> Volver</RouterLink>
    </div>
    <Heading>{{ titulo }}</Heading>
    <div class="mx-auto mt-10 bg-white shadow-sm">
      <div class="mx-auto md:w-2/3 py-20 px-6">
        <FormKit
          type="form"
          submit-label="Guardar Cambios"
          incomplete-message="No se pudo enviar, revisa los mensajes de error"
          :value="formData"
          @submit="handleSubmit">
          <FormKit
            type="text"
            label="Nombre"
            name="nombre"
            placeholder="Nombre del cliente"
            validation="required"
            :validation-messages="{
              required: 'El Nombre del Cliente es obligatorio'
            }"
            v-model="formData.nombre" />
          <FormKit
            type="text"
            label="Apellido"
            name="apellido"
            placeholder="Apellido del cliente"
            validation="required"
            :validation-messages="{
              required: 'El Apellido del Cliente es obligatorio'
            }"
            v-model="formData.apellido" />
          <FormKit
            type="text"
            label="Email"
            name="email"
            placeholder="Email del cliente"
            validation="required|email"
            :validation-messages="{
              required: 'El Email del Cliente es obligatorio',
              email: 'Coloca un email válido'
            }"
            v-model="formData.email" />
          <FormKit
            type="text"
            label="Telefono"
            name="telefono"
            placeholder="Telefono: XXX-XXX-XXXX"
            validation="*matches:/^[0-9]{3}-[0-9]{3}-[0-9]{4}/"
            :validation-messages="{
              matches: 'El el formato del telefono es incorrecto'
            }"
            v-model="formData.telefono" />
          <FormKit
            type="text"
            label="Empresa"
            name="empresa"
            placeholder="Empresa de cliente"
            v-model="formData.empresa" />
          <FormKit
            type="text"
            name="puesto"
            label="Puesto"
            placeholder="Puesto de cliente"
            v-model="formData.puesto" />
          <!-- <FormKit type="submit" label="Agregar Cliente" /> -->
        </FormKit>
      </div>
    </div>
  </div>
</template>
<style>
.formkit-wrapper {
  max-width: 100%;
}
</style>
