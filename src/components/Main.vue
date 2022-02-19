<template>
  <div class="p-5">
    <div>
      <div style="padding-top: 7vh">
        <div v-if="tabIndex == 0" >
          <div>
            <div>
              <p class="h2 text-dark">What master degree are you applying for?</p>
              <div class="content">
                <b-form-textarea
                    placeholder="e.g. Masters of Data Science program at University of Girona"
                    rows="1"
                    v-model="target_job_model"
                ></b-form-textarea>

              </div>
            </div>
          </div>
        </div>
        <div v-else-if="tabIndex == 1">
          <div>
            <div>
              <p class="h2 text-dark">Choose your top 3 strengths.</p>
              <div class="content">
                <b-form-tags id="tags-with-dropdown" v-model="value" no-outer-focus class="mb-2" :limit="limit" >
                  <template v-slot="{ tags, disabled, addTag, removeTag }" >
                    <ul v-if="tags.length > 0" class="list-inline d-inline-block mb-2" style="padding: 2vh 0px 2vh 0px">
                      <li v-for="tag in tags" :key="tag" class="list-inline-item">
                        <b-form-tag
                            @remove="removeTag(tag)"
                            :title="tag"
                            :disabled="disabled"
                            variant="orange"
                            tag-pills
                            size="lg"
                        >{{ tag }}</b-form-tag>
                      </li>
                    </ul>

                    <b-dropdown :disabled="value.length >= limit" variant="outline-orange" block menu-class="w-100" style="padding-bottom: 3vh">
                      <template #button-content>
                        <b-icon icon="tag-fill"></b-icon> Choose tags
                      </template>

                      <b-dropdown-form @submit.stop.prevent="() => {}" class="w-100">
                        <b-form-group
                            label="Search tags:"
                            label-for="tag-search-input"
                            content-cols-="8"
                            label-cols="4"
                            class="mb-0"
                            :description="searchDesc"
                            :disabled="disabled"
                            cols
                        >
                          <b-form-input
                              v-model="search"
                              id="tag-search-input"
                              type="search"
                              size="sm"
                              autocomplete="off"
                          ></b-form-input>
                        </b-form-group>
                      </b-dropdown-form>

                      <b-dropdown-divider class="w-100"></b-dropdown-divider>

                      <template v-if="value.length < limit">
                        <b-dropdown-item-button
                            v-for="option in availableOptions"
                            :key="option"
                            class="w-25"
                            @click="onOptionClick({ option, addTag })
                          "

                        >
                          {{ option }}
                        </b-dropdown-item-button>

                        <b-dropdown-text v-if="availableOptions.length === 0" class="w-100">
                          There are no tags available to select
                        </b-dropdown-text>

                      </template>
                      <b-dropdown-text v-else class="w-100">
                        Delete tags first
                      </b-dropdown-text>


                    </b-dropdown>
                  </template>
                </b-form-tags>
              </div>

            </div>
          </div>
        </div>
        <div v-else-if="tabIndex == 2">
          <div>
            <div>
              <p class="h2 text-dark">How many years of experience do you have?</p>

              <label style="padding-top: 1vh">
                <b class="text-orange"> Years of experience: </b>
                {{years_of_experience_to_string}}


              </label>
              <div class="content">
                <b-form-input variant="orange" id="range-1" v-model="years_of_experience_model" type="range" min="0" max="6"></b-form-input>

              </div>
            </div>


          </div></div>
        <div v-else-if="tabIndex == 3">
          <div>
            <p class="h2 text-dark">What’s your working style?</p>
            <div>
              <b-list-group style="padding-top: 0.5vh">
                <b-row class="align-self-stretch p-2">
                  <b-col>
                    <b-list-group-item href="#" class="align-items-start h-100" @click="working_style = 0" variant="orange" :active="working_style == 0">
                      <div class=" w-100 justify-content-between">
                        <h5 class="mb-1 text-dark">ARTISTIC</h5>
                      </div>

                      <p class="mb-1 text-dark">
                        You thrive in dynamic environments driven by innovation and creativity.
                      </p>

                    </b-list-group-item>
                  </b-col>
                  <b-col>
                    <b-list-group-item variant="orange" href="#" class="align-items-start h-100" @click="working_style = 1" :active="working_style == 1">
                      <div class=" w-100 justify-content-between">
                        <h5 class="mb-1 text-dark">ENTERPRISING</h5>
                      </div>

                      <p class="mb-1 text-dark">
                        You're accostumed to leading teams with empowering and decisive task delegation.
                      </p>

                    </b-list-group-item>
                  </b-col>
                </b-row>
                <b-row class="align-self-stretch p-2">
                  <b-col>
                    <b-list-group-item variant="orange" href="#" class="align-items-start h-100" @click="working_style = 2" :active="working_style == 2">
                      <div class=" w-100 justify-content-between">
                        <h5 class="mb-1 text-dark">LOGICAL</h5>
                      </div>

                      <p class="mb-1 text-dark">
                        You bring a resourceful approach with a knack for problem-solving.
                      </p>

                    </b-list-group-item>
                  </b-col>
                  <b-col>
                    <b-list-group-item variant="orange" href="#" class="align-items-start h-100" @click="working_style = 3" :active="working_style == 3">
                      <div class=" w-100 justify-content-between">
                        <h5 class="mb-1 text-dark">ORGANIZED</h5>
                      </div>

                      <p class="mb-1 text-dark">
                        You bring structure and focus to streamline tasks.
                      </p>

                    </b-list-group-item>
                  </b-col>
                </b-row>
                <b-row class="align-self-stretch p-2">
                  <b-col>
                    <b-list-group-item variant="orange" href="#" class="align-items-start h-100" @click="working_style = 4" :active="working_style == 4">
                      <div class="text-dark w-100 justify-content-between">
                        <h5 class="mb-1">PRACTICAL</h5>
                      </div>

                      <p class="mb-1 text-dark">
                        You go above and beyond to meets goals and ensure timely tasks completion.
                      </p>

                    </b-list-group-item>
                  </b-col>
                  <b-col>
                    <b-list-group-item variant="orange" href="#" class="align-items-start h-100" @click="working_style = 5" :active="working_style == 5">
                      <div class=" w-100 justify-content-between">
                        <h5 class="mb-1 text-dark">SERVICE-ORIENTED</h5>
                      </div>

                      <p class="mb-1 text-dark">
                        You excel in collaborative situations and enjoy helping others.
                      </p>

                    </b-list-group-item>
                  </b-col>
                </b-row>
              </b-list-group>

            </div>
          </div>
        </div>

      </div>


      <b-row style="padding: 5vh 2vw 0px 2vw">
        <b-col>
          <b-btn block @click="tabIndex--" :disabled="tabIndex == 0" variant="dark" squared class="shadow-none"><span class="text-orange">Previous</span></b-btn>
        </b-col>
        <b-col>
          <b-btn v-if="tabIndex < 3" block @click="tabIndex++" :disabled="continue_button_disabled()" variant="dark" squared class="shadow-none"><span class="text-orange">Continue</span></b-btn>
          <b-btn v-else block @click="generateCoverLetter" :disabled="continue_button_disabled()" variant="dark" squared class="shadow-none"><span class="text-orange">GENERATE</span></b-btn>

        </b-col>
      </b-row>

    </div>
  </div>
</template>

<script>

const { Configuration, OpenAIApi } = require("openai");

const configuration = new Configuration({
  apiKey: "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",
});
const openai = new OpenAIApi(configuration);

import { saveAs } from 'file-saver';

export default {
  name: 'Main',
  data() {
    return {
      tabIndex: 0,
      options: ['Collaboration', 'Communication', 'Critical Thinking', 'Customer Service', 'Decision-making', 'Delegation', 'Innovation', 'Interpersonal', 'Leadership', 'Management', 'Motivation', 'Observation', 'Organization', 'Planning', 'Problem-solving', 'Team-building', 'Teamwork', 'Time-management', 'Hard-working'],
      value: [],
      search: '',
      limit: 3,
      years_of_experience_model: 0,
      working_style: null,
      target_job_model: null,


    }
  },
  computed: {
    years_of_experience_to_string(){
      if(this.years_of_experience_model == 0) return "No experience"
      else if(this.years_of_experience_model > 5) return "More than 5"
      else return this.years_of_experience_model
    },
    criteria() {
      // Compute the search criteria
      return this.search.trim().toLowerCase()
    },
    availableOptions() {
      const criteria = this.criteria
      // Filter out already selected options
      const options = this.options.filter(opt => this.value.indexOf(opt) === -1)
      if (criteria) {
        // Show only options that match criteria
        return options.filter(opt => opt.toLowerCase().indexOf(criteria) > -1);
      }
      // Show all options available
      return options
    },
    searchDesc() {
      if (this.criteria && this.availableOptions.length === 0) {
        return 'There are no tags matching your search criteria'
      }
      return ''
    }  },
  methods: {

    async generateCoverLetter(){

      let job_position = this.target_job_model
      let skills = this.value
      const response = await openai.createCompletion("text-davinci-001", {
        prompt: "-Masters of Family Psychology program at the Psychology department at The University of Tennessee \n-Skills: time-management skills, analytical skills and strong research and writing skills\n\nCover letter:\n\nIt is my pleasure to submit my application for the Masters of Family Psychology program at the Psychology department at The University of Tennessee at Chattanooga. As a recent graduate of Belhaven University with a bachelor's degree in Psychology, this graduate program is the perfect next step toward achieving my goal of becoming a Licensed Family and Marriage Counselor and opening my own practice.\n\nDuring my time in the undergraduate program at Belhaven University, I was able to develop several skills that make me a strong candidate for your program including time-management skills, analytical skills and strong research and writing skills. A few of my greatest accomplishments achieved while in my undergraduate program include interning for the esteemed counseling program at The University of Tennessee and graduating with high honors. I have chosen the Masters of Family Psychology program at The University of Tennessee at Chattanooga because of the university's core focus on family counseling within the master's program.\n\nThank you for your time and consideration. Please don't hesitate to contact me if you have any questions or if there is any additional information or documents I can provide. I look forward to hearing from you soon.\n\n-"+job_position+"\n-Skills: "+skills+"\n\nCover letter:",
        temperature: 0.41,
        max_tokens: 1548,
        top_p: 1,
        frequency_penalty: 0,
        presence_penalty: 0,
      });
      let blob = new Blob([response.data.choices[0].text],
          { type: "text/plain;charset=utf-8" });

      saveAs(blob, "static.txt");
    },

    continue_button_disabled(){
      if(this.tabIndex == 0) return this.target_job_model=="" || this.target_job_model==null
      else if(this.tabIndex == 1) return this.value.length == 0
      else if(this.tabIndex == 3) return this.working_style == null
    },
    onOptionClick({ option, addTag }) {
      addTag(option)
      this.search = ''
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.content {
  padding: 4vh 50px 30px 50px;
}
.btn:hover {
  text-decoration: underline !important;
  text-decoration-color: #F4A261 !important;

}
.btn:disabled {
  text-decoration: none !important;
}

.active {
  background-color: #F4A261 !important;
  border-color: rgba(0, 0, 0, 0.125) !important;
}

</style>
