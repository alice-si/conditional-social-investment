<template>
    <md-drawer class="md-right" :md-active.sync="active">
        <md-snackbar :md-active.sync="success">Your positions have been merged!</md-snackbar>
        <div>
            <form novalidate class="md-layout">
                <md-card class="md-layout-item ">
                    <md-card-header style="background-color: rgb(80, 174, 84);">
                        <h3 class="md-title" style="color: #fff;">Merge selected position</h3>
                    </md-card-header>

                    <md-card-content>
                        <div class="md-layout-item md-small-size-100">
                            <md-field>
                                <label for="amount">Amount to merge</label>
                                <md-input name="amount" id="amount" v-model="tx.amount" :disabled="merging"/>
                            </md-field>
                        </div>
                    </md-card-content>
                    <md-progress-bar md-mode="indeterminate" v-if="merging"/>
                    <md-card-actions>
                        <md-button class="md-primary"
                                   :disabled="merging"
                                   @click="merge">Merge
                        </md-button>
                    </md-card-actions>
                </md-card>
            </form>
        </div>
    </md-drawer>
</template>

<script>

  import state from "@/state";

  export default {
    props: [
      'active',
      'project'
    ],
    methods: {
      async merge() {
        this.merging = true;
        console.log('Merging...');
        console.log(this.selectedToMerge);
        let condition = this.selectedToMerge[0].condition;
        console.log(condition);
        await condition.merge(this.selectedToMerge, this.tx.amount);
        this.success = true;
        this.$emit('success');
        this.merging = false;
      }
    },
    data: function () {
      return {
        tx: {
          amount: 0
        },
        selectedToMerge: state.selectedToMerge,
        merging: false,
        success: false,
        showSidepanel: true
      }
    }
  };
</script>
