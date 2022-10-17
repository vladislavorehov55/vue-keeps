<template>
  <div v-if="isOpenLeftPanel">
    <ul class="list">
      <li :class="['listItem', {activeListItem: activeLeftPanelListItemInd === 0}]"
          @click="$emit('setActiveLeftPanelListItemInd', 0)"
      >
        <SVGGenerator name="home-note"/>
        <span class="title">Заметки</span>
      </li>
      <li :class="['listItem', {activeListItem: activeLeftPanelListItemInd === 1}]"
          @click="$emit('setActiveLeftPanelListItemInd', 1)"
      >
        <SVGGenerator name="archive-note"/>
        <span class="title">Архив</span>
      </li>
      <li :class="['listItem', {activeListItem: activeLeftPanelListItemInd === 2}]"
          @click="$emit('setActiveLeftPanelListItemInd', 2)"
      >
        <SVGGenerator name="trash-note"/>
        <span class="title">Корзина</span>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "LeftPanel",
  props: {
    isOpenLeftPanel: Boolean,
    activeLeftPanelListItemInd: Number || null
  },


}
</script>

<style scoped>
.list {
  width: 280px;
  list-style: none;
  height: 100%;
  position: fixed;
  /*left: -280px;*/
  z-index: 1;
  border-right: 1px solid black;
  background: white;
  padding-top: 5px;
}
.listItem {
  display: flex;
  align-items: center;
  width: 100%;
  height: 48px;
  padding-left: 10px;
  cursor: pointer;
  border-top-right-radius: 100px;
  border-bottom-right-radius: 100px;
}
.listItem:hover {
  background: #f1f3f4;
}
.activeListItem {
  background: #feefc3!important;
}
.title {
  margin-left: 20px;
}
</style>