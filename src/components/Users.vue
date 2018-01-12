<template lang="pug">
    .users__container
      input.users__search(type="text" name="search" placeholder="Search" @input="searchHandler")
      transition-group(name="fade" tag="table").users
          tr.users__info(:key="231")
              td(@click="sort('name')") Name
              td(@click="sort('location')") Location
              td(@click="sort('currency')") Currency
          tr.users__item(v-for="item in items" :key="item.id")
              td.users__item__name {{ item.name }}
              td.users__item__location {{ item.location }}
              td.users__item__currency {{ item.currency }}
          tr.users__total(:key="2312")
              td(colspan="2") Total
              td {{ total }}
</template>

<script>
    import fetchData from '../api/test.json';

    export default {
      name: 'Users',
      data() {
        return {
          id: 2,
          items: [...fetchData],
          total: fetchData.reduce((sum, item) => sum + item.currency, 0),
        };
      },
      methods: {
        searchHandler(e) {
          const value = e.target.value.toLowerCase();
          this.items = fetchData.filter(el =>
              el.name.toLowerCase().search(value) + 1 || el.location.toLowerCase().search(value) + 1);
          this.total = this.items.reduce((sum, item) => sum + item.currency, 0);
        },
        sort(byProp) {
          function compare(a, b) {
            if (a[byProp] < b[byProp]) return -1;
            if (a[byProp] > b[byProp]) return 1;
            return 0;
          }
          return this.items.sort(compare);
        },
      },
    };
</script>

<style lang="sass">
    .users
        max-width: 500px
        margin-left: auto
        margin-right: auto
        border: 1px solid #eee
        text-align: left
        border-collapse: collapse
        margin-bottom: 100px
        width: 454px
        td
            border: 1px solid #eee
            padding: 10px 15px
            &:nth-child(3)
                text-align: center
        &__search
            width: 454px
            max-width: 100%
            margin-bottom: 15px
            margin-left: auto
            margin-right: auto
            border: none
            border-bottom: 2px solid #eee
            font-size: 18px
            padding-bottom: 10px
            outline: none
            transition: .3s
            &:focus
                border-bottom: 2px solid gold
        &__info
            font-weight: bold
            background: #eee
            cursor: pointer
            td:hover
                background: #eee
        &__total
            font-weight: bold
            td:last-child
                text-align: center
                background: #eee
    .fade-enter-active, .fade-leave-active
        transition: all .4s
        border: 1px solid #eee
    .fade-enter, .fade-leave-to
        transform: translateX(30px)
</style>
