<template>
  <div>
    <p id="header_2">ВАЖНО ЗНАТЬ О...</p>
    <guarantee/>
    <delivery-info/>
    <guide/>
    <toy-care/>
    <user-agreement/>
  </div>
</template>
<script>
import Guarantee from '../info/Guarantee'
import Guide from '../info/Guide'
import ToyCare from '../info/ToyCare'
import DeliveryInfo from '../info/DeliveryInfo'
import UserAgreement from '../info/UserAgreement'

export default {
  name: 'contacts-important',
  components: {UserAgreement, DeliveryInfo, ToyCare, Guarantee, Guide}
}
</script>
<style scoped lang="scss">

  #header_2 {
    font-size: 13px;
    font-family: $secondary-font;
    letter-spacing: 1px;
    color: white;
    margin-bottom: 5px;
  }

  #header_2:after {
    content: "";
    display: block;
    height: 1px;
    border-bottom: 1px solid $color-secondary;
    width: 120px;
  }

  .sub_header {
    font-family: $primary-font;
    font-weight: 600;
    color: $color-info;
    font-size: 12px;
    line-height: 22px;
  }
</style>
