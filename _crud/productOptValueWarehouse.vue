<template></template>

<script>
export default {
  data() {
    return {
      crudId: this.$uid(),
      productOptionValues: []
    }
  },
  computed: {
    crudData() {
      return {
        crudId: this.crudId,
        entityName: config("main.qcommerce.entityNames.productoptionvaluewarehouses"),
        apiRoute: 'apiRoutes.qcommerce.productOptionValueWarehouse',
        permission: 'icommerce.productoptionvaluewarehouses',
        extraFormFields: 'Icommerce.crud-fields.productoptionvaluewarehouses',
        create: {
          title: this.$tr('icommerce.cms.form.warehouseByOptions'),
        },
        read: {
          help: {
            title: this.$tr('icommerce.cms.form.warehouseByOptions'),
            icon: 'fa-light fa-warehouse-full',
            description: this.$tr('icommerce.cms.form.warehouseByOptionsHelp')
          },
          columns: [
            {name: 'id', label: this.$tr('isite.cms.form.id'), field: 'id', style: 'width: 50px'},
            {
              name: 'option', label: this.$tr('isite.cms.form.option'), field: 'productOptionValue',
              format: val => val.option.description, align: 'left',
            },
            {
              name: 'value', label: this.$tr('icommerce.cms.form.optionValue'), field: 'productOptionValue',
              format: val => val.optionValue.description, align: 'left',
            },
            {
              name: 'warehouse',
              label: this.$tr('icommerce.cms.label.warehouse'),
              field: 'warehouse',
              align: 'left',
              format: val => val.title || ''
            },
            {name: 'quantity', label: this.$tr('isite.cms.form.quantity'), field: 'quantity', align: 'left'},
            {
              name: 'created_at', label: this.$tr('isite.cms.form.createdAt'), field: 'createdAt', align: 'left',
              format: val => val ? this.$trd(val) : '-',
            },
            {name: 'actions', label: this.$tr('isite.cms.form.actions'), align: 'left'},
          ],
          requestParams: {
            include: 'warehouse',
            filter: {},
          },
          filters: {
            productOptionValueId: {
              value: null,
              type: 'select',
              required: true,
              props: {
                label: `${this.$tr('icommerce.cms.form.optionValue')}*`
              },
              loadOptions: {
                apiRoute: 'apiRoutes.qcommerce.productOptionValues',
                requestParams: {include: 'option,optionValue'},
                select: {label: item => `${item.option.description} - ${item.optionValue}`, id: 'id'}
              }
            },
            warehouseId: {
              value: null,
              type: 'select',
              required: true,
              props: {
                label: `${this.$tr('icommerce.cms.label.warehouse')}*`
              },
              loadOptions: {
                apiRoute: 'apiRoutes.qcommerce.warehouses'
              }
            }
          }
        },
        update: {
          title: this.$tr('icommerce.cms.updateWarehouse'),
          requestParams: {include: 'warehouse'}
        },
        delete: true,
        formLeft: {
          id: {value: ''},
          userId: {value: this.$store.state.quserAuth.userId},
          optionId: {value: null},
          optionValueId: {value: null},
          productId: {
            value: null,
            type: 'select',
            required: true,
            props: {
              label: `${this.$tr('isite.cms.form.product')}*`
            },
            loadOptions: {
              apiRoute: 'apiRoutes.qcommerce.products',
              select: {label: 'name', id: 'id'}
            }
          },
          productOptionValueId: {
            value: null,
            type: 'select',
            props: {
              label: `${this.$tr('icommerce.cms.form.optionValue')}`,
              clearable: true
            },
            loadOptions: {
              apiRoute: 'apiRoutes.qcommerce.productOptionValues',
              requestParams: {include: 'option,optionValue'},
              select: {label: item => `${item.option.description} - ${item.optionValue}`, id: 'id'},
              loadedOptions: (data) => this.productOptionValues = data
            }
          },
          warehouseId: {
            value: null,
            type: 'select',
            props: {
              label: `${this.$tr('icommerce.cms.label.warehouse')}`,
              clearable: true
            },
            loadOptions: {
              apiRoute: 'apiRoutes.qcommerce.warehouses'
            }
          },
          quantity: {
            value: '',
            type: 'input',
            required: true,
            props: {
              label: `${this.$tr('isite.cms.form.quantity')}*`,
            }
          }
        },
        getDataForm: (data, type) => {
          return new Promise(resolve => {
            //Set the optionId and OptionValueId
            let prodOptionValue = this.productOptionValues.find(item => item.id == data.productOptionValueId)
            if (prodOptionValue) {
              data.optionId = prodOptionValue.optionId
              data.optionValueId = prodOptionValue.optionValueId
            }

            //Response
            resolve(data)
          })
        }
      }
    },
    //Crud info
    crudInfo() {
      return this.$store.state.qcrudComponent.component[this.crudId] || {}
    }
  }
}
</script>
