<template>
  <app-layout>
    <template #title>
      {{  title }}s List
    </template>
    <template #actions>
      <inertia-link
        :href="create_url"
        class='btn btn-success'
      >
        Create {{  title }}
      </inertia-link>
    </template>

    <contact-table
      :url="url"
      :is-vendor="is_vendor"
      :is-customer="is_customer"
    ></contact-table>
  </app-layout>
</template>

<script>
import AppLayout from "../../Layouts/AppLayout";
import ContactTable from "../../Components/Contact/ContactTable";
export default {
  name: "Index",
  props: {
    create_url: {
      default: "",
      type: String,
    },
    url: {
      default: "",
      type: String,
    },
    title: {
      default: "",
      type: String,
    },
    is_vendor: {
      default: "",
      type: String,
    },
    is_customer: {
      default: "",
      type: String,
    },
  },
  components: { AppLayout, ContactTable },
};
</script>

<style scoped>
</style>
