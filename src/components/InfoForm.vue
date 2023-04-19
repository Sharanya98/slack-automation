<template>
    <v-card max-width="500" height="500" class="mx-auto my-16 bg-teal-darken-1">
        <v-app-bar class="text-center font-weight-bold" color="teal-darken-4" title="Oyster Slack Automation">

        </v-app-bar>
        <v-main>

            <v-sheet width="300" class="mx-auto" color="teal-darken-1">
                <v-form @submit.prevent>
                    <v-text-field v-model="firstName" label="First name" variant="solo"></v-text-field>
                    <v-text-field v-model="lastName" label="Last name" variant="solo"></v-text-field>
                    <v-combobox label="please select" v-model="selectedCompany" :items="company"
                        variant="solo"></v-combobox>

                    <div class="text-center">
                        <v-btn @click="submit" class="text-center text-subtitle-1" color="teal-lighten-4" size="large"
                            variant="flat">
                            Press Hanko
                        </v-btn>
                    </div>
                </v-form>
                <v-card v-show="getdata" class="mt-4 pa-4" width="300">
                    <p><span class="font-weight-bold">{{ getname }}</span> send hanko to <span class="font-weight-bold">{{
                        selectedCompany }}</span> at <span class="font-weight-bold"> {{ time }}</span> on
                        <span class="font-weight-bold">{{ date }}</span>
                    </p>
                </v-card>
            </v-sheet>
        </v-main>
    </v-card>
</template>
<script>

export default {
    name: 'infoform',
    data: () => ({

        firstName: '',
        lastName: '',
        date: '',
        time: '',
        selectedCompany: [],
        getdata: false,
        company: ['company 1', 'company 2', 'company 3', 'company 4', 'company 5', 'company 6']
    }),
    computed: {
        getname() {
            return this.firstName + ' ' + this.lastName
        }
    },
    methods: {
        async submit() {

            this.getdata = true
            var currentDate = new Date();
            currentDate = currentDate.toLocaleString('en-US', { hour: 'numeric', minute: 'numeric', hour12: true })
            this.time = currentDate
            console.log(currentDate);

            var currentDateWithFormat = new Date().toJSON().slice(0, 10).replace(/-/g, '/');
            this.date = currentDateWithFormat
            console.log(currentDateWithFormat);
            await this.addRole()

           




        },
        async addRole() {
           
           
            var url = "https://sheetdb.io/api/v1/esbtfhbovb5z9"
            
            var options = {
                method: "POST",
                headers: { "Content-Type": "application/json" },
                body: JSON.stringify({
                    FirstName: this.firstName,
                    LastName: this.lastName,
                    Date: this.date,
                    Time: this.time,
                    Company: this.selectedCompany
                })
            }
            let response = await fetch(url,options)
            let result = await response.json()

            if(result.error){
                console.log("error",result.error)
            }
            else{
                console.log("success",result)
            }
        },

    },

}

</script>