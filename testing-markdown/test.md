# Vue demo

<script>
Vue.component("listify", {
  props: ["list", "listindex"],
  template: `
  <div class='list-container'>
    <h1>{{list.title}}</h1>
    <div v-show="list.items.length > 0">
        <table class="table list">
            <tr v-for="(item, itemIndex) in applyFilterSelect">
                <td>
                    <input type="checkbox" :checked="item.completed" v-model="item.completed">
                    <label class="text-gray-900 font-bold text-xl mb-2" @click="">{{item.title}}</label>
                    <input type="text" v-model="item.title" />
                </td>
                <td>
                    <button class="flex-shrink-0 bg-red-500 hover:bg-red-700 border-red-500 hover:border-red-700 text-sm border-4 text-white py-1 px-2 rounded" @click="deleteItem(listindex, item)">Remove</button>
                </td>
            </tr>
            <tr v-show="newItem">
                <td colspan="2">{{newItem}}</td>
            </tr>
        </table>
    </div>
    <form class="w-full max-w-sm" @submit.prevent="addNewItem">
        <div class="flex items-center border-b border-b-2 border-teal-500 py-2">
            <input class="appearance-none bg-transparent border-none w-full text-gray-700 mr-3 py-1 px-2 leading-tight focus:outline-none" type="text" placeholder="Add new ingredient item" v-model="newItem" aria-label="Ingredient Item">
            <button class="flex-shrink-0 bg-teal-500 hover:bg-teal-700 border-teal-500 hover:border-teal-700 text-sm border-4 text-white py-1 px-2 rounded" type="submit"> ADD
            </button>
        </div>
    </form>
  </div>
  `,
  data() {
    return {
      newItem: "",
      editItem: null,
      filterPicked: "all",
      remainingTotal: 0
    };
  },
  mounted() {
    // console.log(this)
  },
  methods: {
    addNewItem() {
      if (this.newItem) {
        this.$emit("additem", this.newItem, this.listindex);
        this.newItem = "";
      }
    },
    deleteItem(arrayIndex, itemIndex) {
      this.$emit("removeitem", arrayIndex, itemIndex);
    }
  },
  computed: {
    applyFilterSelect() {
      if (this.filterPicked === "completed") {
        return this.list.items.filter(a => a.completed);
      } else if (this.filterPicked === "notcompleted") {
        return this.list.items.filter(a => !a.completed);
      }
      return this.list.items;
    },
    remaining() {
      return this.list.items.reduce(function(total, next) {
        if (next.completed === false) {
          return total + 1;
        } else {
          return total;
        }
      }, 0);
    }
  }
});

new Vue({
  el: "#app-two",
  data() {
    return {
      lists: [
        {
          title: "Opener",
          options: { filters: true },
          items: [
            { title: "OJ", completed: false },
            { title: "MILK", completed: false },
            { title: "VODKA", completed: true },
            { title: "RUM", completed: true },
            { title: "HONEY", completed: false },
            { title: "SOMETHING", completed: true }
          ]
        }
      ]
    };
  },
  methods: {
    addNewItem(item, arrayIndex) {
      this.lists[arrayIndex].items.push({ title: item, completed: false });
    },
    deleteItem(arrayIndex, todo) {
      this.lists[arrayIndex].items.splice(
        this.lists[arrayIndex].items.indexOf(todo),
        1
      );
    }
  }
});

</script>

### Above is the component script for the app div below

<div id="app-two">
    <div v-for="(list, listIndex) in lists">
        <div class="container">
            <listify :listIndex="listIndex" :list="list" @additem="addNewItem" @removeItem="deleteItem" />
        </div>
    </div>
</div>

<hr>

<div class="max-w-sm w-full lg:max-w-full lg:flex">
    <div class="h-48 lg:h-auto lg:w-48 flex-none bg-cover rounded-t lg:rounded-t-none lg:rounded-l text-center overflow-hidden" style="background-image: url('./assets/images/cocktail.jpg')" title="mojito">
    </div>
    <div class="border-r border-b border-l border-gray-400 lg:border-l-0 lg:border-t lg:border-gray-400 bg-white rounded-b lg:rounded-b-none lg:rounded-r p-4 flex flex-col justify-between leading-normal">
        <div class="mb-8">
            <p class="text-sm text-gray-600 flex items-center">
                Craft Cocktail
            </p>
            <div class="text-gray-900 font-bold text-xl mb-2">Mojito</div>
            <p class="text-gray-700 text-base">Lorem ipsum dolor sit amet, consectetur adipisicing elit. Voluptatibus quia, nulla! Maiores et perferendis eaque, exercitationem praesentium nihil.</p>
        </div>
        <div class="flex items-center">
            <img class="w-10 h-10 rounded-full mr-4" src="./assets/images/avatar.jpg" alt="Avatar of Jovani">
            <div class="text-sm">
                <p class="text-gray-900 leading-none">Jovani</p>
                <p class="text-gray-600">Aug 18</p>
            </div>
        </div>
    </div>
</div>

<hr>

<div class="p-12 flex flex-wrap items-center justify-center">
<div class="flex-1 m-8 rounded-lg border border-gray-400 lg:border lg:border-gray-400 bg-gray-100 shadow-lg">
<div class="md:flex p-4">
    <div>
        <img class="rounded-lg h-48 w-48 object-cover" src="./assets/images/cocktail.jpg" alt="mojito">
    </div>
    <div class="mt-4 md:mt-0 md:ml-6">
        <div class="uppercase tracking-wide text-lg text-purple-800 font-bold">Test</div>
        <a href="#" class="block mt-2 text-md leading-tight font-semibold text-gray-900 hover:underline">Finding customers for your new business</a>
        <p class="mt-2 text-gray-600">Getting a new business off the ground is a lot of hard work. Here are five ideas you can use to find your first customers.</p>
    </div>
</div>
</div>

<div class="flex-1 m-8 rounded-lg border border-gray-400 lg:border lg:border-gray-400 bg-gray-100 shadow-lg">
<div class="md:flex p-4">
    <div>
        <img class="rounded-lg h-48 w-48 object-cover" src="./assets/images/cocktail.jpg" alt="mojito">
    </div>
    <div class="mt-4 md:mt-0 md:ml-6">
        <div class="uppercase tracking-wide text-lg text-purple-800 font-bold">Test</div>
        <a href="#" class="block mt-2 text-md leading-tight font-semibold text-gray-900 hover:underline">Finding customers for your new business</a>
        <p class="mt-2 text-gray-600">Getting a new business off the ground is a lot of hard work. Here are five ideas you can use to find your first customers.</p>
    </div>
</div>
</div>

<div class="flex-1 m-8 rounded-lg border border-gray-400 lg:border lg:border-gray-400 bg-gray-100 shadow-lg">
<div class="md:flex p-4">
    <div class="">
        <img class="rounded-lg h-48 w-48 object-cover" src="./assets/images/cocktail.jpg" alt="mojito">
    </div>
    <div class="mt-4 md:mt-0 md:ml-6">
        <div class="uppercase tracking-wide text-lg text-purple-800 font-bold">Test</div>
        <a href="#" class="block mt-2 text-md leading-tight font-semibold text-gray-900 hover:underline">Finding customers for your new business</a>
        <p class="mt-2 text-gray-600">Getting a new business off the ground is a lot of hard work. Here are five ideas you can use to find your first customers.</p>
    </div>
</div>
</div>
</div>

<hr>

### Testing out the component cards

<div class="p-12 flex flex-wrap items-center justify-center">
    <div class="flex-shrink-0 m-6 relative overflow-hidden bg-orange-500 rounded-lg max-w-xs shadow-lg">
        <svg class="absolute bottom-0 left-0 mb-8" viewBox="0 0 375 283" fill="none" style="transform: scale(1.5); opacity: 0.1;">
            <rect x="159.52" y="175" width="152" height="152" rx="8" transform="rotate(-45 159.52 175)" fill="white" />
            <rect y="107.48" width="152" height="152" rx="8" transform="rotate(-45 0 107.48)" fill="white" />
        </svg>
        <div class="relative pt-10 px-10 flex items-center justify-center">
            <div class="block absolute w-48 h-48 bottom-0 left-0 -mb-24 ml-3" style="background: radial-gradient(black, transparent 60%); transform: rotate3d(0, 0, 1, 20deg) scale3d(1, 0.6, 1); opacity: 0.2;"></div>
            <img class="relative w-40" src="./assets/images/cocktail.jpg" alt="mojito">
        </div>
        <div class="relative text-white px-6 pb-6 mt-6">
            <span class="block opacity-75 -mb-1">Craft</span>
            <div class="flex justify-between">
                <span class="block font-semibold text-xl">White Lily</span>
                <span class="block bg-white rounded-full text-orange-500 text-xs font-bold px-3 py-2 leading-none flex items-center">SAVE</span>
            </div>
        </div>
    </div>
    <div class="flex-shrink-0 m-6 relative overflow-hidden bg-teal-500 rounded-lg max-w-xs shadow-lg">
        <svg class="absolute bottom-0 left-0 mb-8" viewBox="0 0 375 283" fill="none" style="transform: scale(1.5); opacity: 0.1;">
            <rect x="159.52" y="175" width="152" height="152" rx="8" transform="rotate(-45 159.52 175)" fill="white" />
            <rect y="107.48" width="152" height="152" rx="8" transform="rotate(-45 0 107.48)" fill="white" />
        </svg>
        <div class="relative pt-10 px-10 flex items-center justify-center">
            <div class="block absolute w-48 h-48 bottom-0 left-0 -mb-24 ml-3" style="background: radial-gradient(black, transparent 60%); transform: rotate3d(0, 0, 1, 20deg) scale3d(1, 0.6, 1); opacity: 0.2;"></div>
            <img class="relative w-40" src="./assets/images/cocktail.jpg" alt="mojito">
        </div>
        <div class="relative text-white px-6 pb-6 mt-6">
            <span class="block opacity-75 -mb-1">Craft</span>
            <div class="flex justify-between">
                <span class="block font-semibold text-xl">Manhattan</span>
                <span class="block bg-white rounded-full text-teal-500 text-xs font-bold px-3 py-2 leading-none flex items-center">SAVE</span>
            </div>
        </div>
    </div>
    <div class="flex-shrink-0 m-6 relative overflow-hidden bg-purple-500 rounded-lg max-w-xs shadow-lg">
        <svg class="absolute bottom-0 left-0 mb-8" viewBox="0 0 375 283" fill="none" style="transform: scale(1.5); opacity: 0.1;">
            <rect x="159.52" y="175" width="152" height="152" rx="8" transform="rotate(-45 159.52 175)" fill="white" />
            <rect y="107.48" width="152" height="152" rx="8" transform="rotate(-45 0 107.48)" fill="white" />
        </svg>
        <div class="relative pt-10 px-10 flex items-center justify-center">
            <div class="block absolute w-48 h-48 bottom-0 left-0 -mb-24 ml-3" style="background: radial-gradient(black, transparent 60%); transform: rotate3d(0, 0, 1, 20deg) scale3d(1, 0.6, 1); opacity: 0.2;"></div>
            <img class="relative w-40" src="./assets/images/cocktail.jpg" alt="mojito">
        </div>
        <div class="relative text-white px-6 pb-6 mt-6">
            <span class="block opacity-75 -mb-1">Classic</span>
            <div class="flex justify-between">
                <span class="block font-semibold text-xl">Mai Tai</span>
                <span class="block bg-white rounded-full text-purple-500 text-xs font-bold px-3 py-2 leading-none flex items-center">SAVE</span>
            </div>
        </div>
    </div>
</div>

<hr>

<div class="p-12 flex flex-wrap items-center justify-center">
    <div class="min-w-32 bg-white min-h-48 p-3 mb-4 font-medium">
        <div class="w-32 flex-none rounded-t lg:rounded-t-none lg:rounded-l text-center shadow-lg ">
            <div class="block rounded-t overflow-hidden  text-center ">
                <div class="bg-blue text-white py-1">
                    WHISKEY
                </div>
                <div class="pt-1 border-l border-r border-white bg-white">
                    <span class="text-5xl font-bold leading-tight">
                TEXT
              </span>
                </div>
                <div class="border-l border-r border-b rounded-b-lg text-center border-white bg-white -pt-2 -mb-1">
                    <span class="text-sm">
                TEXT
              </span>
                </div>
                <div class="pb-2 border-l border-r border-b rounded-b-lg text-center border-white bg-white">
                    <span class="text-xs leading-normal">
                TEXT
              </span>
                </div>
            </div>
        </div>
    </div>
    <div class="min-w-32 bg-white min-h-48 p-3 mb-4 font-medium">
        <div class="w-32 flex-none rounded-t lg:rounded-t-none lg:rounded-l text-center shadow-lg ">
            <div class="block rounded-t overflow-hidden  text-center ">
                <div class="bg-blue text-white py-1">
                    WHISKEY
                </div>
                <div class="pt-1 border-l border-r border-white bg-white">
                    <span class="text-5xl font-bold leading-tight">
                TEXT
              </span>
                </div>
                <div class="border-l border-r border-b rounded-b-lg text-center border-white bg-white -pt-2 -mb-1">
                    <span class="text-sm">
                TEXT
              </span>
                </div>
                <div class="pb-2 border-l border-r border-b rounded-b-lg text-center border-white bg-white">
                    <span class="text-xs leading-normal">
                TEXT
              </span>
                </div>
            </div>
        </div>
    </div>
    <div class="min-w-32 bg-white min-h-48 p-3 mb-4 font-medium">
        <div class="w-32 flex-none rounded-t lg:rounded-t-none lg:rounded-l text-center shadow-lg ">
            <div class="block rounded-t overflow-hidden  text-center ">
                <div class="bg-blue text-white py-1">
                    WHISKEY
                </div>
                <div class="pt-1 border-l border-r border-white bg-white">
                    <span class="text-5xl font-bold leading-tight">
                TEXT
              </span>
                </div>
                <div class="border-l border-r border-b rounded-b-lg text-center border-white bg-white -pt-2 -mb-1">
                    <span class="text-sm">
                TEXT
              </span>
                </div>
                <div class="pb-2 border-l border-r border-b rounded-b-lg text-center border-white bg-white">
                    <span class="text-xs leading-normal">
                TEXT
              </span>
                </div>
            </div>
        </div>
    </div>
</div>

<hr>

### Avatars

<style>
    .line-height-username1 {
        line-height: 3.75rem;
    }
    
    .line-height-username2 {
        line-height: 5.5rem;
    }
    
    .line-height-username3 {
        line-height: 7.5rem;
    }
</style>
<div>
    <div class="inline-block mb-6 rounded-full bg-gray-300 pr-5 h-16 line-height-username1">
        <img class="rounded-full float-left h-full" src="./assets/images/avatar.jpg" alt="Avatar of Jovani"> <span class="ml-3">Jovani</span>
    </div>
    <div></div>
    <div class="inline-block mb-6 rounded-full bg-gray-300 pr-6 h-24 line-height-username2 text-3xl">
        <img class="rounded-full float-left h-full" src="./assets/images/avatar.jpg" alt="Avatar of Jovani"> <span class="ml-3">Jovani</span>
    </div>
    <div></div>
    <div class="inline-block rounded-full bg-gray-300 h-32 pr-8 line-height-username3 text-5xl">
        <img class="rounded-full float-left h-full" src="./assets/images/avatar.jpg" alt="Avatar of Jovani"> <span class="ml-4">Jovani</span>
    </div>
</div>

<hr>

<div class="max-w-6xl mx-auto">
  <div class="flex items-center justify-center min-h-screen">
    <div class="max-w-sm w-full sm:w-1/2 lg:w-1/3 py-6 px-3">
      <div class="bg-white shadow-xl rounded-lg overflow-hidden">
        <div class="bg-cover bg-center h-56 p-4" style="background-image: url(./assets/images/cocktail.jpg)">
          <div class="flex justify-end">
            <svg class="h-6 w-6 text-white fill-current" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24">
              <path d="M12.76 3.76a6 6 0 0 1 8.48 8.48l-8.53 8.54a1 1 0 0 1-1.42 0l-8.53-8.54a6 6 0 0 1 8.48-8.48l.76.75.76-.75zm7.07 7.07a4 4 0 1 0-5.66-5.66l-1.46 1.47a1 1 0 0 1-1.42 0L9.83 5.17a4 4 0 1 0-5.66 5.66L12 18.66l7.83-7.83z"></path>
            </svg>
          </div>
        </div>
        <div class="p-4">
          <p class="uppercase tracking-wide text-sm font-bold text-gray-700">RUM • SWEET</p>
          <p class="text-3xl text-gray-900">MAI TAI</p>
          <p class="text-gray-700">SUB TITLE ???</p>
        </div>
        <div class="flex p-4 border-t border-gray-300 text-gray-700">
          <div class="flex-1 inline-flex items-center">
            <p><span class="text-gray-900 font-bold">RUM • SWEET• SWEET• SWEET• SWEET</span></p>
          </div>
          <div class="flex-1 inline-flex items-center">
            <p><span class="text-gray-900 font-bold">OTHER</span></p>
          </div>
        </div>
        <div class="px-4 pt-3 pb-4 border-t border-gray-300 bg-gray-100">
          <div class="text-xs uppercase font-bold text-gray-600 tracking-wide">Creator</div>
          <div class="flex items-center pt-2">
            <div class="bg-cover bg-center w-10 h-10 rounded-full mr-3" style="background-image: url(./assets/images/avatar.jpg)">
            </div>
            <div>
              <p class="font-bold text-gray-900">Jovani</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>

---

<h5>CLASSIC COCKTAIL</h5>
<h2 class="mb-4">RITA</h2>
<div class="h-full w-full max-h-screen top-0 flex-wrap items-center justify-center">
    <img class="relative w-360 h-64 m-16" src="./assets/images/cocktail.jpg" alt="mojito">
    <div class="flex flex-row mx-2 mb-8">
        <div class="w-full md:w-1/2 lg:w-2/3 px-2 mb-4">
            <div class="border h-12 text-sm text-grey-dark flex items-center justify-center">
                <p>Details of the cocktail and some table talk.</p>
            </div>
        </div>
        <div class="w-full md:w-1/2 lg:w-1/3 px-2 mb-4">
            <div class="border h-12 text-sm text-grey-dark flex items-center justify-center">
                <img class="w-10 h-10 rounded-full mr-4" src="./assets/images/avatar.jpg" alt="Avatar of Jovani">
                <img class="w-10 h-10 rounded-full mr-4" src="./assets/images/avatar.jpg" alt="Avatar of Jovani">
                <img class="w-10 h-10 rounded-full mr-4" src="./assets/images/avatar.jpg" alt="Avatar of Jovani">
                <img class="w-10 h-10 rounded-full mr-4" src="./assets/images/avatar.jpg" alt="Avatar of Jovani">
            </div>
        </div>
    </div>
    <div class="flex flex-row -mx-2 mb-8">
        <div class="w-full md:w-1/2 lg:w-1/4 px-2 mb-4 flex">
            <div class="border h-12 text-sm text-grey-dark flex flex-col items-center justify-center">
                <h5 class="mb-4">GLASS</h5>
                <p>full / half / quarter</p>
            </div>
        </div>
        <div class="w-full md:w-1/2 lg:w-1/4 px-2 mb-4 flex">
            <div class="border h-12 text-sm text-grey-dark flex flex-col items-center justify-center">
                <h5 class="mb-4">GARNISH</h5>
                <p>full / half / quarter</p>
            </div>
        </div>
        <div class="w-full md:w-1/2 lg:w-1/4 px-2 mb-4 flex">
            <div class="border h-12 text-sm text-grey-dark flex flex-col items-center justify-center">
                <h5 class="mb-4">OCCASION</h5>
                <p>full / half / quarter</p>
            </div>
        </div>
        <div class="w-full md:w-1/2 lg:w-1/4 px-2 mb-4 flex">
            <div class="border h-12 text-sm text-grey-dark flex flex-col items-center justify-center">
                <h5 class="mb-4">TAGS</h5>
                <p>full / half / quarter</p>
            </div>
        </div>
    </div>
    <div class="flex flex-row -mx-2 mb-8">
        <div class="border w-full md:w-1/2 lg:w-1/2 px-2 mb-4 flex flex-col">
            <div class="h-12 text-sm text-grey-dark flex">
                <h3 class="text-3xl font-bold text-center lg:text-left">Ingredients:</h1>
                <ul class="mt-4 list-disc">
                    <li class="mb-4">Mango infused</li>
                    <li class="mb-4">Milk</li>
                    <li class="mb-4">Juice</li>
                    <li class="mb-4">Lime Juice</li>
                    <li class="mb-4">OJ</li>
                </ul>
            </div>
        </div>
        <div class="border w-full md:w-1/2 lg:w-1/2 px-2 mb-4 flex flex-col">
            <div class="h-12 text-sm text-grey-dark flex">
                <h3 class="text-3xl font-bold text-center lg:text-left">Method:</h1>
                <ul class="mt-4">
                    <li class="mb-4"><b>One</b>: Lorem ipsum dolor sit amet, eum accumsan cotidieque instructior in.</li>
                    <li class="mb-4"><b>Two</b>: Lorem ipsum dolor sit amet, eum accumsan cotidieque instructior in.</li>
                    <li class="mb-4"><b>Three</b>: Lorem ipsum dolor sit amet, eum accumsan cotidieque instructior in.</li>
                </ul>
            </div>
        </div>
    </div>
  </div>

---

<div class="md:flex bg-white rounded-lg p-6">
    <img class="h-16 w-16 md:h-24 md:w-24 rounded-full mx-auto md:mx-0 md:mr-6"  src="./assets/images/avatar.jpg">
    <div class="text-center md:text-left">
         <h2 class="text-lg">Title</h2>
         <div class="text-purple-500">Text</div>
         <div class="text-gray-600">Text</div>
         <div class="text-gray-600">Numbers</div>
    </div>
</div>

---

<div class="hover:bg-blue-800 hover:text-white">
    <a href="#" class="relative block py-6 px-4 lg:p-6 text-sm lg:text-base font-bold hover:bg-blue-800 hover:text-white">Hover</a>
    <div class="p-6 mega-menu mb-16 sm:mb-0 shadow-xl bg-blue-800">
        <div class="container mx-auto w-full flex flex-wrap justify-between mx-2">
            <div class="w-full text-white mb-8">
                <h2 class="font-bold text-2xl">Main Hero Message for the menu section</h2>
                <p>Sub-hero message, not too long and not too short. Make it just right!</p>
            </div>
            <ul class="px-4 w-full sm:w-1/2 lg:w-1/4 border-gray-600 border-b sm:border-r lg:border-b-0 pb-6 pt-6 lg:pt-3">
                <div class="flex items-center">
                    <svg class="h-8 mb-3 mr-3 fill-current text-white" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20">
                        <path d="M3 6c0-1.1.9-2 2-2h8l4-4h2v16h-2l-4-4H5a2 2 0 0 1-2-2H1V6h2zm8 9v5H8l-1.67-5H5v-2h8v2h-2z" />
                    </svg>
                    <h3 class="font-bold text-xl text-white text-bold mb-2">Heading 1</h3>
                </div>
                <p class="text-gray-100 text-sm">Quarterly sales are at an all-time low create spaces to explore the accountable talk and blind vampires.</p>
                <div class="flex items-center py-3">
                    <svg class="h-6 pr-3 fill-current text-blue-300" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20">
                        <path d="M20 10a10 10 0 1 1-20 0 10 10 0 0 1 20 0zm-2 0a8 8 0 1 0-16 0 8 8 0 0 0 16 0zm-8 2H5V8h5V5l5 5-5 5v-3z" />
                    </svg>
                    <a href="#" class="text-white bold border-b-2 border-blue-300 hover:text-blue-300">Find out more...</a>
                </div>
            </ul>
            <ul class="px-4 w-full sm:w-1/2 lg:w-1/4 border-gray-600 border-b sm:border-r-0 lg:border-r lg:border-b-0 pb-6 pt-6 lg:pt-3">
                <div class="flex items-center">
                    <svg class="h-8 mb-3 mr-3 fill-current text-white" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20">
                        <path d="M4.13 12H4a2 2 0 1 0 1.8 1.11L7.86 10a2.03 2.03 0 0 0 .65-.07l1.55 1.55a2 2 0 1 0 3.72-.37L15.87 8H16a2 2 0 1 0-1.8-1.11L12.14 10a2.03 2.03 0 0 0-.65.07L9.93 8.52a2 2 0 1 0-3.72.37L4.13 12zM0 4c0-1.1.9-2 2-2h16a2 2 0 0 1 2 2v12a2 2 0 0 1-2 2H2a2 2 0 0 1-2-2V4z" />
                    </svg>
                    <h3 class="font-bold text-xl text-white text-bold mb-2">Heading 2</h3>
                </div>
                <p class="text-gray-100 text-sm">Prioritize these line items game-plan draw a line in the sand come up with something buzzworthy UX upstream selling.</p>
                <div class="flex items-center py-3">
                    <svg class="h-6 pr-3 fill-current text-blue-300" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20">
                        <path d="M20 10a10 10 0 1 1-20 0 10 10 0 0 1 20 0zm-2 0a8 8 0 1 0-16 0 8 8 0 0 0 16 0zm-8 2H5V8h5V5l5 5-5 5v-3z" />
                    </svg>
                    <a href="#" class="text-white bold border-b-2 border-blue-300 hover:text-blue-300">Find out more...</a>
                </div>
            </ul>
            <ul class="px-4 w-full sm:w-1/2 lg:w-1/4 border-gray-600 border-b sm:border-b-0 sm:border-r md:border-b-0 pb-6 pt-6 lg:pt-3">
                <div class="flex items-center">
                    <svg class="h-8 mb-3 mr-3 fill-current text-white" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20">
                        <path d="M2 4v14h14v-6l2-2v10H0V2h10L8 4H2zm10.3-.3l4 4L8 16H4v-4l8.3-8.3zm1.4-1.4L16 0l4 4-2.3 2.3-4-4z" />
                    </svg>
                    <h3 class="font-bold text-xl text-white text-bold mb-2">Heading 3</h3>
                </div>
                <p class="text-gray-100 text-sm">This proposal is a win-win situation which will cause a stellar paradigm shift, let's touch base off-line before we fire the new ux experience.</p>
                <div class="flex items-center py-3">
                    <svg class="h-6 pr-3 fill-current text-blue-300" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20">
                        <path d="M20 10a10 10 0 1 1-20 0 10 10 0 0 1 20 0zm-2 0a8 8 0 1 0-16 0 8 8 0 0 0 16 0zm-8 2H5V8h5V5l5 5-5 5v-3z" />
                    </svg>
                    <a href="#" class="text-white bold border-b-2 border-blue-300 hover:text-blue-300">Find out more...</a>
                </div>
            </ul>
            <ul class="px-4 w-full sm:w-1/2 lg:w-1/4 border-gray-600 pb-6 pt-6 lg:pt-3">
                <div class="flex items-center">
                    <svg class="h-8 mb-3 mr-3 fill-current text-white" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20">
                        <path d="M9 12H1v6a2 2 0 0 0 2 2h14a2 2 0 0 0 2-2v-6h-8v2H9v-2zm0-1H0V5c0-1.1.9-2 2-2h4V2a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v1h4a2 2 0 0 1 2 2v6h-9V9H9v2zm3-8V2H8v1h4z" />
                    </svg>
                    <h3 class="font-bold text-xl text-white text-bold mb-2">Heading 4</h3>
                </div>
                <p class="text-gray-100 text-sm">This is a no-brainer to wash your face, or we need to future-proof this high performance keywords granularity.</p>
                <div class="flex items-center py-3">
                    <svg class="h-6 pr-3 fill-current text-blue-300" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20">
                        <path d="M20 10a10 10 0 1 1-20 0 10 10 0 0 1 20 0zm-2 0a8 8 0 1 0-16 0 8 8 0 0 0 16 0zm-8 2H5V8h5V5l5 5-5 5v-3z" />
                    </svg>
                    <a href="#" class="text-white bold border-b-2 border-blue-300 hover:text-blue-300">Find out more...</a>
                </div>
            </ul>
        </div>
    </div>
</div>
