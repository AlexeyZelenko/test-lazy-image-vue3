<template>
	<div>
		<div align="right" class="mb-2">
			<b-button :variant='Primary' v-on:click="addMeal" class="mr-2">Add Meal</b-button>
			<b-button :variant='Primary' v-on:click="removeMeal">Remove Meal</b-button>
		</div>
		<b-table :items="items" :fields="fields" striped responsive="sm">

			<template #cell(Info)="row">
				<b-button size="sm" @click="row.toggleDetails" class="mr-2">
					{{ row.detailsShowing ? 'Hide' : 'Show'}} Details
				</b-button>

				<b-button v-b-modal.edit size="sm" v-on:click="selected_row = row.item">Edit</b-button>
				<b-modal id="edit" title="BootstrapVue">
					<ModalDiet :rowData="selected_row"/>
				</b-modal>

			</template>
			<template #row-details="row">
				<b-card>
					<div class="row">

						<b-col>
							<b-row class="mb-2">
								<b-col sm="3" class="text-sm-right"><b>Protein</b></b-col>
								<b-col>{{ row.item.Protein_Products }}</b-col>
							</b-row>

							<b-row class="mb-2">
								<b-col sm="3" class="text-sm-right"><b>Carbs:</b></b-col>
								<b-col>{{ row.item.Carbs_Products }}</b-col>
							</b-row>

							<b-row class="mb-2">
								<b-col sm="3" class="text-sm-right"><b>Fats:</b></b-col>
								<b-col>{{ row.item.Fats_Products }}</b-col>
							</b-row>
						</b-col>
						<b-col>
							<b-row class="mb-2" v-for="item in [p_grams, c_grams, f_grams]" :key="item">
								<b-col sm="3" class="text-sm-right"><b>grams:</b></b-col>
								<b-col>{{ item }}</b-col>
							</b-row>
						</b-col>

					</div>
				</b-card>
			</template>
		</b-table>
	</div>
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
  import ModalDiet from "./ModalDiet.vue"
  import { defineAsyncComponent, defineComponent, ref } from "vue"
  import LazyImageVue3 from 'lazy-image-vue3'

  export default defineComponent({
    name: 'App',
    data: () => ({
      // items: [
      //   {
      //     img: 'https://cdn-images-1.medium.com/max/800/1*xjGrvQSXvj72W4zD6IWzfg.jpeg',
      //   },
      //   {
      //     img: 'https://cdn-images-1.medium.com/max/800/1*xjGrvQSXvj72W4zD6IWzfg.jpeg'
      //   },
      //   {
      //     img: 'https://cdn-images-1.medium.com/max/800/1*xjGrvQSXvj72W4zD6IWzfg.jpeg'
      //   },
      //   {
      //     img: 'https://cdn-images-1.medium.com/max/800/1*xjGrvQSXvj72W4zD6IWzfg.jpeg'
      //   },
      //   {
      //     img: 'https://cdn-images-1.medium.com/max/800/1*xjGrvQSXvj72W4zD6IWzfg.jpeg'
      //   },
      //   {
      //     img: 'https://cdn-images-1.medium.com/max/800/1*xjGrvQSXvj72W4zD6IWzfg.jpeg'
      //   },
      //   {
      //     img: 'https://cdn-images-1.medium.com/max/800/1*xjGrvQSXvj72W4zD6IWzfg.jpeg'
      //   }
      // ],
      protein_products: [],
      p_grams: null,
      carbs_products: [],
      c_grams: null,
      fats_products: [],
      f_grams: null,
      fields: ["Meal", "Protein/g", "Carbs/g", "Fats/g", "Kcal", "Info"],
      items: [
      ],
      selected_row: null
    }),
    methods: {
      addMeal() {
        if (this.items.length < 5) {
          const meal_obj = {
            "Meal": this.items.length + 1, "Protein/g": "",
            "Carbs/g": "",
            "Fats/g": "",
            "Kcal": "",
            Protein_Products: this.protein_products,
            Carbs_Products: this.carbs_products,
            Fats_Products: this.fats_products
          }
          this.items.push(meal_obj)
        } else {
          alert("5 meals max")
        }
      },
      removeMeal() {
        if (this.items.length > 0) {
          this.items.pop()
        }
      },
    },
    components: {
      ModalDiet,
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
