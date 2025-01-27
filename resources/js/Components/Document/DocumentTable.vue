<template>
  <div>
    <div class="card-header">
      <div class="d-flex align-items-center justify-content-between">

        <div>
          <input
            type="text"
            class="form-control form-control-sm"
            placeholder="search..."
            v-model="searching"
          />
        </div>
      </div>
    </div>
    <data-grid-frame
      @pageChanged="pageChanged"
      :loading="loading"
      :items="items"
      :paginator-info="paginatorInfo"
    >
      <template v-slot:title></template>
      <template v-slot:rows>
        <data-grid-column
          label="Number"
          props="document_number"
        >
          <template v-slot:default="{ item }">
            {{ item.document_number }}
          </template>
        </data-grid-column>
        <data-grid-column
          :label="`${contactTitle} Name`"
          props="contact_name"
        >

        </data-grid-column>

        <data-grid-column
          label="Amount"
          props="amount"
        >

        </data-grid-column>

        <data-grid-column
          label="Issued At"
          props="issued_at"
        >

        </data-grid-column>
        <data-grid-column
          label="Due At"
          props="due_at"
        >

        </data-grid-column>
        <data-grid-column
          label="Status"
          props="status"
        >

        </data-grid-column>
        <data-grid-column
          label="Created At"
          props="created_at"
        >

        </data-grid-column>
        <data-grid-column label="Option">
          <template v-slot:default="{ item }">
            <el-dropdown>
              <button class="btn btn-primary btn-sm">
                Manage
              </button>
              <template #dropdown>
                <el-dropdown-menu>

                  <inertia-link :href="`${url}/${item.id}/edit`">
                    <el-dropdown-item>Edit</el-dropdown-item>
                  </inertia-link>

                </el-dropdown-menu>
              </template>
            </el-dropdown>
          </template>
        </data-grid-column>
      </template>
    </data-grid-frame>
  </div>
</template>

<script>
import DataGridFrame from "../Frame/DataGridFrame";
import DataGridColumn from "../Frame/DataGridColumn";
import PrimaryButton from "../Button/PrimaryButton.vue";
import { useQuery, useResult } from "@vue/apollo-composable";
import gql from "graphql-tag";
import { ref } from "vue";
export default {
  name: "DocumentTable",
  components: { DataGridColumn, PrimaryButton, DataGridFrame },
  props: {
    url: {
      default: "",
      type: String,
    },
    type: {
      default: "",
      type: String,
    },
  },
  setup(props) {
    const page = ref(1);
    const searching = ref("");
    const { result, loading } = useQuery(
      gql`
        query getDocuments($page: Int!, $search: String!, $type: String) {
          getDocuments(page: $page, search: $search, type: $type) {
            data {
              id
              document_number
              order_number
              status
              currency_code
              issued_at
              due_at
              issued_at
              currency_rate
              amount
              contact_name
              created_at
            }
            paginatorInfo {
              count
              perPage
              currentPage
              total
            }
          }
        }
      `,
      {
        page: page,
        search: searching,
        type: props.type,
      }
    );
    const items = useResult(result, [], (data) => data.getDocuments.data);

    const paginatorInfo = useResult(
      result,
      {},
      (data) => data.getDocuments.paginatorInfo
    );

    function pageChanged(currentPage) {
      page.value = currentPage;
    }

    return {
      contactTitle: props.type === "BILL" ? "Vendor" : "Customer",
      searching,
      pageChanged,
      items,
      paginatorInfo,
      loading,
    };
  },
};
</script>
