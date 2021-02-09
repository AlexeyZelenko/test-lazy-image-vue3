<template>
	<div
			style="height: 500px"
			v-for="(item, i) in items"
			:key="i"
	>

		<LazyImageVue3 :picture="item.img"/>
	</div>

</template>

<script>
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
  components: {
    LazyImageVue3
  }
}
</script>

<style>

</style>
