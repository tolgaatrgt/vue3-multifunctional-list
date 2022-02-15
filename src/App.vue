<template>
  <div :class="$style['app-container']">
    <div>
      <SearchBar
        :isMatch="Boolean(matchedValue.length)"
        @checkInputText="checkInputText"
        @addItem="addItem" />
      <List
        v-if="itemList.length"
        :matchedValue="matchedValue"
        :items="itemList"
        @removeItem="removeItem" />
      <span v-else :class="$style['info-message']">No Items</span>
    </div>
    <SortWidget
      :selectedSortType="selectedSortType"
      @sortByValue="sortByValue"
      @sortByDate="sortByDate" />
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, watch } from 'vue';
import List from './components/List.vue';
import SearchBar from './components/SearchBar.vue';
import SortWidget from './components/SortWidget.vue';
import IconButton from './components/IconButton.vue';
import { IListItem } from './types';
import { setItemList, getItemList, updateItemList } from './store';
export default defineComponent({
  name: 'App',
  components: {
    List,
    SearchBar,
    SortWidget,
    IconButton,
  },
  setup() {
    const itemList = ref<IListItem[]>([]);
    const selectedSortType = ref('value');
    const matchedValue = ref('');
    itemList.value = getItemList();

    const sortByValue = () => {
      selectedSortType.value = 'value';
      itemList.value.sort((a, b) => a.value.localeCompare(b.value));
    };

    const sortByDate = () => {
      selectedSortType.value = 'date';
      itemList.value.sort(
        (a, b) =>
          new Date(b.createdAt).valueOf() - new Date(a.createdAt).valueOf()
      );
    };

    const checkInputText = (text: string) => {
      const trimmedText = text.trim();
      if (
        itemList.value &&
        itemList.value.some(
          (item) =>
            item.value.trim().toLowerCase() === trimmedText.toLowerCase()
        )
      ) {
        matchedValue.value = trimmedText;
      } else matchedValue.value = '';
    };

    const addItem = (text: string) => {
      if (!matchedValue.value) {
        setItemList(text);
        itemList.value = getItemList();
      } else return;
    };
    const removeItem = (idx: number) => {
      const newItemList = itemList.value.filter((item, i) => i !== idx);
      itemList.value = newItemList;
      updateItemList(newItemList);
    };
    sortByValue();

    watch(
      [selectedSortType, itemList],
      ([sortType, prevSortType], [items, prevItems]) => {
        if (sortType === 'value') {
          sortByValue();
        }
        if (sortType === 'date') {
          sortByDate();
        }
      }
    );

    return {
      itemList,
      sortByValue,
      sortByDate,
      checkInputText,
      addItem,
      removeItem,
      selectedSortType,
      matchedValue,
    };
  },
});
</script>

<style lang="scss" module>
@import './sass/_color.scss';

.app-container {
  padding: 100px 0;
  height: 100vh;
  display: flex;
  justify-content: center;
  background-color: $bg-primary;
  position: relative;
}

.info-message {
  display: block;
  color: $gray;
  font-style: italic;
  width: 100%;
  text-align: center;
}
</style>
