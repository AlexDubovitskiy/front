<script>
import Shipment from "@/components/orders/Shipment";
import UiCheckbox from "../base-components/ui-checkbox"
import UiInput from "../base-components/ui-input"
import UiSelect from "../base-components/ui-select"
import BaseButton from "../base-components/BaseButton";

export default {
  name      : "Content",
  components: {
    Shipment,
    UiCheckbox,
    UiInput,
    BaseButton,
    UiSelect
  },
  computed  : {},
  methods   : {
    checkAvailableOrders() {
      if(this.orders.chosenOrderObjects){
        //Отправка формы
      }
    },
    pushToChosenOrders(orderId) {
      if (!this.orders.chosenOrdersIdx.includes(orderId)) {
        this.removeByAttr(this.orders.chosenOrderObjects, 'id', orderId)
      } else {
        if (this.orders.availableOrders[orderId]['city_from'] && this.orders.availableOrders[orderId]['city_to']) {
          this.orders.chosenOrderObjects.push(this.orders.availableOrders[orderId]);
        }
      }
    },
    removeByAttr(arr, attr, value) {
      let i = arr.length;
      while (i--) {
        if (arr[i] && arr[i].hasOwnProperty(attr) && (arguments.length > 2 && arr[i][attr] === value)) {
          arr.splice(i, 1);
        }
      }
      return arr;
    }
  },
  data() {
    return {
      orders: {
        availableOrders   : {
          box_1: {
            id       : 'box_1',
            name     : 'box_name_1',
            weight   : '100',
            number   : '1',
            city_to  : '',
            city_from: '',
          },
          box_2: {
            id       : 'box_2',
            name     : 'box_name_2',
            weight   : '200',
            number   : '2',
            city_to  : '',
            city_from: '',
          },
          box_3: {
            id       : 'box_3',
            name     : 'box_name_3',
            weight   : '300',
            number   : '3',
            city_to  : '',
            city_from: '',
          },
        },
        chosenOrdersIdx   : [],
        chosenOrderObjects: [],
        cities            : ['spb', 'msk', 'ekt', 'ny'],
      },
    }
  }
}
</script>

<template>
  <div class = "orders">

    <section class = "section">
      <div class = "orders-dashboard">
        <span class = "orders-dashboard__title">Available orders</span>
        <div class = "orders-dashboard__row orders-dashboard__row--header">
          <div class = "orders-dashboard__column orders-dashboard__column--head">Boxes</div>
          <div class = "orders-dashboard__column orders-dashboard__column--head">From</div>
          <div class = "orders-dashboard__column orders-dashboard__column--head">To</div>
        </div>

        <template v-for = "(order, idx) in orders.availableOrders">
          <div class = "orders-dashboard__row">
            <div class = "orders-dashboard__column">
              <ui-checkbox
                  :key = "idx"
                  :value = "order.id"
                  v-model:model-value = "orders.chosenOrdersIdx"
                  :disabled="!order.city_from || !order.city_to"
                  @callback = "pushToChosenOrders(order.id); checkAvailableOrders()">
                {{ order.name }}
              </ui-checkbox>
            </div>
            <div class = "orders-dashboard__column">
              <ui-select
                  :key = "idx"
                  :list = "orders.cities"
                  v-model = "order.city_from"/>
            </div>
            <div class = " orders-dashboard__column">
              <ui-select
                  :key = "idx"
                  :list = "orders.cities"
                  v-model = "order.city_to"/>
            </div>
          </div>
        </template>
      </div>
      <hr class = "section__hr">
    </section>
<!--    {{ orders.availableOrders }}-->
    <section class = "section">
      <div class = "selected-orders">
        <span class = "selected-orders__title" v-if="orders.chosenOrderObjects.length > 0">You selected</span>
        <span class = "selected-orders__title" v-else>Nothing selected</span>
        <template v-for = "order in orders.chosenOrderObjects">
          <div class = "selected-orders-order">
            <div class = "selected-orders-order__name">{{ order.name }}</div>
            <div class = "selected-orders-order__direction"> |</div>
            <div class = "selected-orders-order__direction selected-orders-order__direction--from">
              {{ order.city_from }}
            </div>
            <div class = "selected-orders-order__direction"> -</div>
            <div class = "selected-orders-order__direction selected-orders-order__direction--to">
              {{ order.city_to }}
            </div>
          </div>
        </template>
      </div>
    </section>

    <section class = "section">
      <div class = "road-info">
        <div class = "available-trucks">
          <span class = "available-trucks__title">Available trucks</span>
          <div class = "available-trucks__name">Kamaz</div>
        </div>
        <div class = "available-drivers">
          <span class = "available-drivers__title">Available drivers</span>
          <div class = "available-drivers__name">Ivanov A.A.</div>
        </div>
      </div>
      <hr class = "section__hr">
    </section>

    <BaseButton
        :button = "{
        name: 'Create order',
        class: 'orders__btn w-25 btn btn-primary'
    }"/>
  </div>
</template>

<style scoped lang = "scss" src = "../../styles/pages/orders.scss"></style>