<template>
  <div class="text-center">
    <v-dialog v-model="dialog" width="700">
      <template v-slot:activator="{ on, attrs }">
        <v-btn color="accent" dark v-bind="attrs" v-on="on"> {{ $t("recipe.api-extras") }} </v-btn>
      </template>

      <v-card>
        <v-card-title> {{ $t("recipe.api-extras") }} </v-card-title>

        <v-card-text :key="formKey">
          <v-row align="center" v-for="(value, key, index) in extras" :key="index">
            <v-col cols="12" sm="1">
              <v-btn fab text x-small color="white" elevation="0" @click="removeExtra(key)">
                <v-icon color="error">{{ $globals.icons.delete }}</v-icon>
              </v-btn>
            </v-col>
            <v-col cols="12" md="3" sm="6">
              <v-text-field :label="$t('recipe.object-key')" :value="key" @input="updateKey(index)"> </v-text-field>
            </v-col>
            <v-col cols="12" md="8" sm="6">
              <v-text-field :label="$t('recipe.object-value')" v-model="extras[key]"> </v-text-field>
            </v-col>
          </v-row>
        </v-card-text>

        <v-divider></v-divider>

        <v-card-actions>
          <v-form ref="addKey">
            <v-text-field
              :label="$t('recipe.new-key-name')"
              v-model="newKeyName"
              class="pr-4"
              :rules="[rules.required, rules.whiteSpace]"
            ></v-text-field>
          </v-form>
          <v-btn color="info" text @click="append"> {{ $t("recipe.add-key") }} </v-btn>

          <v-spacer></v-spacer>

          <v-btn color="success" text @click="save"> {{ $t("general.save") }} </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
export default {
  props: {
    extras: Object,
  },
  data() {
    return {
      newKeyName: null,
      dialog: false,
      formKey: 1,
      rules: {
        required: v => !!v || this.$i18n.t("recipe.key-name-required"),
        whiteSpace: v => !v || v.split(" ").length <= 1 || this.$i18n.t("recipe.no-white-space-allowed"),
      },
    };
  },

  methods: {
    save() {
      this.$emit("save", this.extras);
      this.dialog = false;
    },
    append() {
      if (this.$refs.addKey.validate()) {
        this.extras[this.newKeyName] = "value";
        this.formKey += 1;
      }
    },
    removeExtra(key) {
      delete this.extras[key];
      this.formKey += 1;
    },
  },
};
</script>

<style></style>
