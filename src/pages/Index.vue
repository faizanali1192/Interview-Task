<template>
  <q-page class="q-px-sm">
    <!-- Add Parent Dialogs -->
    <q-dialog v-model="parentDialog" persistent>
      <q-card style="min-width: 350px">
        <q-card-section>
          <div class="text-h6">Enter Your First List</div>
        </q-card-section>

        <q-card-section class="q-pt-none">
          <q-input dense v-model="title" autofocus @keyup.enter="addParent" />
        </q-card-section>

        <q-card-actions align="right" class="text-primary">
          <q-btn flat label="Cancel" v-close-popup />
          <q-btn flat label="Add" @click="addParent" />
        </q-card-actions>
      </q-card>
    </q-dialog>
    <!-- --- -->

    <!-- Add Items Dialog -->
    <q-dialog v-model="itemDialog" persistent>
      <q-card style="min-width: 350px">
        <q-card-section>
          <div class="text-h6">Your Item</div>
        </q-card-section>

        <q-card-section class="q-pt-none">
          <q-input dense v-model="itemTitle" autofocus @keyup.enter="addItem" />
        </q-card-section>

        <q-card-actions align="right" class="text-primary">
          <q-btn flat label="Cancel" v-close-popup />
          <q-btn flat label="Add" @click="addItem" />
        </q-card-actions>
      </q-card>
    </q-dialog>
    <!-- ------ -->

    <!-- parent list -->

    <div class="absolute-center">
      <q-card
        v-if="mainParent.title"
        bordered
        style="
          max-width: 500px;
          min-width: 500px;
          min-height: 100px;
          max-height: 470px;
          overflow-y: scroll;
          background: #ff7676;
          color: white;
          display: block;
        "
        class="q-pa-md"
      >
        <h5 class="text-center">Enter Todo</h5>
        <!-- New List Field -->
        <div class="row text-h6 items-center">
          <q-input
            v-model="listTitle"
            class="col"
            type="text"
            label="Enter List Title"
            borderless
            dense
            dark
            filled
            square
          />
          <div class="col-auto">
            <q-btn
              color="pink"
              icon="mdi-plus"
              label="Add New"
              @click="addchildList"
            />
          </div>
        </div>
        <!-- ---- -->
        <hr />
        <q-card-section class="text-h6">
          <div class="row">
            <div class="col bordered">
              {{ mainParent.title }}
            </div>
            <div class="col-auto" @click="itemDialog = true">
              <span class="mdi mdi-plus"></span>
            </div>
          </div>
        </q-card-section>
        <q-card-section>
          <q-list
            dense
            class="rounded-borders"
            v-if="mainParent.listItems.length > 0"
          >
            <q-item
              clickable
              v-ripple
              v-for="(v, k) in mainParent.listItems"
              :key="k"
              class="q-ma-sm bordered"
              style="background: #ffa5a5"
            >
              <q-item-section> {{ v }} </q-item-section>
            </q-item>
          </q-list>
        </q-card-section>
        <q-card-section>
          <q-card
            bordered
            v-for="(v, k) in mainParent.listChilds"
            :key="k"
            class="q-mt-md"
            style="background: #ff8080"
          >
            <q-card-section>
              <div class="row">
                <div class="col text-h5">
                  {{ v["title"] }}
                </div>
                <div
                  class="col-auto"
                  @click="
                    itemDialog = true;
                    childIndex = k;
                  "
                >
                  <span class="text-h5 mdi mdi-plus"></span>
                </div>
              </div>
            </q-card-section>
            <q-card-section>
              <q-list
                dense
                padding
                class="rounded-borders"
                v-if="v.listItems.length > 0"
              >
                <hr />
                <q-item
                  clickable
                  v-ripple
                  v-for="(v, k) in v.listItems"
                  :key="k"
                  class="q-ma-sm bordered"
                  style="background: #ffa5a5"
                >
                  <q-item-section> {{ v }} </q-item-section>
                </q-item>
              </q-list>
            </q-card-section>
          </q-card>
        </q-card-section>
      </q-card>
    </div>
    <div
      class="column justify-center items-center window-height text-center"
      v-if="!mainParent.title"
    >
      <h6>By Clicking the Button you can add your First List</h6>
      <q-btn
        @click="parentDialog = true"
        style="background: #ff8080; color: white"
      >
        Add First Parent list
      </q-btn>
    </div>
  </q-page>
</template>

<script>
export default {
  name: "PageIndex",
  data: () => ({
    listTitle: null,
    title: null,
    childIndex: null,
    itemTitle: null,
    itemDialog: false,
    parentDialog: false,
    mainParent: {
      title: null,
      listItems: [],
      listChilds: [],
    },
  }),
  methods: {
    // This Method is used for adding the First parent List.
    addParent() {
      this.mainParent.title = this.title;
      this.parentDialog = false;
    },
    // This Method is used for adding items to List.
    addItem() {
      if (this.childIndex != null) {
        this.mainParent.listChilds[this.childIndex].listItems.push(
          this.itemTitle
        );
      } else {
        this.mainParent.listItems.push(this.itemTitle);
      }
      this.itemDialog = false;
    },
    // This Method is used for adding child list to the parent List.
    addchildList() {
      this.mainParent.listChilds.push({
        title: this.listTitle,
        listItems: [],
      });
      this.listTitle = null;
    },
  },
  watch: {
    //This watcher is used for reseting the values of Item Dialog.
    itemDialog(v) {
      if (!v) {
        this.childIndex = null;
        this.itemTitle = null;
      }
    },
    //This watcher is used for reseting the values of Parent Dialog.
    parentDialog(v) {
      if (!v) this.title = null;
    },
  },
};
</script>
<style scoped>
::-webkit-scrollbar {
  width: 10px;
}

/* Track */
::-webkit-scrollbar-track {
  background: #ff8080;
}

/* Handle */
::-webkit-scrollbar-thumb {
  background: #eb5050;
}

/* Handle on hover */
::-webkit-scrollbar-thumb:hover {
  background: #f05d5d;
}
</style>
