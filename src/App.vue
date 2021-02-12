<template>
<!--	Динамические компоненты-->
	<div>
		<button
				@click="addTag()"
				type="button"
		>
			Add new tag
		</button>
		<br>
		<div>
			<div v-for="child in children" :key="child.name">
				<component :is="child"></component>
			</div>
		</div>
	</div>


<!--	Adding Drag and Drop to Your Vue 3 Project-->
	<div
			class="drop-zone"
			@drop="onDrop($event, 1)"
			@dragenter.prevent
			@dragover.prevent

	>
		<div
				v-for="item in getList(1)"
				:key="item.id"
				@dragstart="startDrag($event, item)"
				class="drag-el"
				draggable="true"
		>
			{{ item.title }}
		</div>
	</div>

	<div
			class="drop-zone"
			@drop="onDrop($event, 2)"
			@dragenter.prevent
			@dragover.prevent
	>
		<div
				v-for="item in getList(2)"
				:key="item.id"
				class="drag-el"
				draggable="true"
				@dragstart="startDrag($event, item)"
		>
			{{ item.title }}
		</div>
	</div>

	<!--	Ленивая загрузка-->
	<div
			:key="i"
			style="height: 500px"
			v-for="(item, i) in items"
	>

		<LazyImageVue3 :picture="item.img"/>
	</div>

</template>

<script>
  import { defineAsyncComponent, defineComponent, ref } from "vue"
  import LazyImageVue3 from 'lazy-image-vue3'

  export default defineComponent({
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
      ],
    }),
    components: {
      LazyImageVue3,
      UploadTaskTag: defineAsyncComponent(() => import("./components/UploadTaskTag.vue"))
    },
    setup() {
      // Динамические компоненты
      const children = ref([])
      const addTag = () => {
        children.value.push('UploadTaskTag')
        console.log(children.value)
      }

      // Adding Drag and Drop to Your Vue 3 Project
      const items = ref([
        {id: 0, title: 'Item A', list: 1},
        {id: 1, title: 'Item B', list: 1},
        {id: 2, title: 'Item C', list: 2}
      ])
      const getList = (list) => {
        return items.value.filter((item) => item.list == list)
      }
      const startDrag = (event, item) => {
        console.log(item)
        event.dataTransfer.dropEffect = 'move'
        event.dataTransfer.effectAllowed = 'move'
        event.dataTransfer.setData('itemID', item.id)
      }
      const onDrop = (event, list) => {
        const itemID = event.dataTransfer.getData('itemID')
        const item = items.value.find((item) => item.id == itemID)
        item.list = list
      }

      return {
        addTag,
        children,
        getList,
        startDrag,
        onDrop
      }
    },
  })
</script>

<style>
	.drop-zone {
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
