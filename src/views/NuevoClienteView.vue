<script setup>
import { useRouter } from 'vue-router'
import { FormKit } from '@formkit/vue'
import RouterLink from '../components/ui/RouterLink.vue'
import Heading from '../components/ui/Heading.vue'
import ClienteService from '../services/ClienteService'
const router = useRouter()
defineProps({
  titulo: {
    type: String
  }
})
const handleSubmit = (data) => {
  data.estado = 1
  ClienteService.agregarCliente(data)
    .then((rest) => router.push({ name: 'listado-clientes' }))
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
          submit-label="Agregar Cliente"
          incomplete-message="No se pudo enviar, revisa los mensajes de error"
          @submit="handleSubmit">
          <FormKit
            type="text"
            label="Nombre"
            name="nombre"
            placeholder="Nombre del cliente"
            validation="required"
            :validation-messages="{
              required: 'El Nombre del Cliente es obligatorio'
            }" />
          <FormKit
            type="text"
            label="Apellido"
            name="apellido"
            placeholder="Apellido del cliente"
            validation="required"
            :validation-messages="{
              required: 'El Apellido del Cliente es obligatorio'
            }" />
          <FormKit
            type="text"
            label="Email"
            name="email"
            placeholder="Email del cliente"
            validation="required|email"
            :validation-messages="{
              required: 'El Email del Cliente es obligatorio',
              email: 'Coloca un email válido'
            }" />
          <FormKit
            type="text"
            label="Telefono"
            name="telefono"
            placeholder="Telefono: XXX-XXX-XXXX"
            validation="*matches:/^[0-9]{3}-[0-9]{3}-[0-9]{4}/"
            :validation-messages="{
              matches: 'El el formato del telefono es incorrecto'
            }" />
          <FormKit
            type="text"
            label="Empresa"
            name="empresa"
            placeholder="Empresa de cliente" />
          <FormKit
            type="text"
            name="puesto"
            label="Puesto"
            placeholder="Puesto de cliente" />
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
