<template>
 <div class="drop-zone">
 <div
 v-for="item in getlist(1)"
 :key="item.id"
 class="drag-el"
 draggable="true"
 >
	{{ item.title }}
 </div>
 </div>
	<div class="drop-zone">
		<div v-for="item in getlist(2)" :key="item.id" class="drag-el">
			{{ item.title }}
		</div>
	</div>

<!--	Ленивая загрузка-->
	<div
			style="height: 500px"
			v-for="(item, i) in items"
			:key="i"
	>

		<LazyImageVue3 :picture="item.img"/>
	</div>

</template>

<script>
	import { ref } from 'vue'
  import LazyImageVue3 from 'lazy-image-vue3'

export default {
  name: 'App',
	data: () => ({
		items: [
			{
			img: 'https://cdn-images-1.medium.com/max/800/1*xjGrvQSXvj72W4zD6IWzfg.jpeg',
			},
      {
        img: 'https://cdn-images-1.medium.com/max/800/1*xjGrvQSXvj72W4zD6IWzfg.jpeg'
      },
      {
        img: 'https://cdn-images-1.medium.com/max/800/1*xjGrvQSXvj72W4zD6IWzfg.jpeg'
      },
      {
        img: 'https://cdn-images-1.medium.com/max/800/1*xjGrvQSXvj72W4zD6IWzfg.jpeg'
      },
      {
        img: 'https://cdn-images-1.medium.com/max/800/1*xjGrvQSXvj72W4zD6IWzfg.jpeg'
      },
      {
        img: 'https://cdn-images-1.medium.com/max/800/1*xjGrvQSXvj72W4zD6IWzfg.jpeg'
      },
      {
        img: 'https://cdn-images-1.medium.com/max/800/1*xjGrvQSXvj72W4zD6IWzfg.jpeg'
      }
		]
  }),
	setup () {
    const items = ref([
			{ id: 0, title: 'Items A', list: 1},
			{ id: 1, title: 'Items B', list: 1},
			{ id: 2, title: 'Items C', list: 2}
		])

		const getlist = (list) => {
      return items.value.filter((item) => item.list = list)
		}

		return {
      getlist
		}
	},
  components: {
    LazyImageVue3
  }
}
</script>

<style>
	.drop-zone{
		width: 50%;
		margin: 50px auto;
		background-color: #3e7c7c;
		padding: 10px;
		min-height: 10px
	}

	.drag-el {
		background-color: #83c1c1;
		padding: 5px;
		margin-bottom: 10px
	}
</style>
