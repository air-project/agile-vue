<template>
  <div class="box">
    <div class="box-header">
      <h4>{{ pageTitle }}</h4>
    </div>
    <admin-group-form
      v-if="adminGroup"
      :submit-text="pageTitle"
      :admin-group="adminGroup"
      :is-edit="true"
      @on-submit="editAdminGroup"
    />
    <placeholder-form v-else :rows="3" />
  </div>
</template>

<script>
import AdminGroupForm from '@admin/views/admin-group/_EditForm'
import AdminGroupService from '@admin/services/AdminGroupService'
import flatry from '@core/utils/flatry'
import PlaceholderForm from '@core/components/Placeholder/PlaceholderForm'

export default {
  components: { PlaceholderForm, AdminGroupForm },
  data() {
    return {
      pageTitle: '编辑管理组',
      adminGroup: null,
    }
  },
  async created() {
    const { data } = await flatry(
      AdminGroupService.edit(this.$router.currentRoute.query.id)
    )

    if (data) {
      this.adminGroup = data
    }
  },
  methods: {
    async editAdminGroup(adminGroup, done, fail, always) {
      const { data, error } = await flatry(
        AdminGroupService.edit(adminGroup.id, adminGroup)
      )

      if (data) {
        done()

        this.$message.success('管理组编辑成功')
      }

      if (error) {
        fail(error)
      }

      always()
    },
  },
}
</script>
