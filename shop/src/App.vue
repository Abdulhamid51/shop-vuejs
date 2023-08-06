<template>
    <div class="m-5 container">
        <div class="row">
            <h1>{{ title }}</h1>
            <div class="col-md-9">
                <input class="form-control" type="text" ref="name" placeholder="Davlat yoki shahar ...">
            </div>
            <div class="col-md-3">
                <button class="btn btn-success" @click="sendMessage">Tasdiqlash</button>
            </div>
        </div>
        <br>
        <div class="row disable" ref="enable">
            <div class="col-1">
                <img v-bind:src="image" alt="icon">
            </div>
            <div class="col-4">
                <h2>{{ country }}</h2>
                <small>{{ small }}</small>
            </div>
            <div class="col-2">
                <h3>C&deg; {{ ce }}</h3>
            </div>
            <div class="col-2">
                <h3>F&deg; {{ fe }}</h3>
            </div>
            <div class="col-2">
                <h2>{{ time }}</h2>
            </div>
        </div>
    </div>
</template>

<script>

export default {
    name: 'App',
    data() {
        return {
            title: 'Ob-havo',
            image: null,
            country: null,
            small: null,
            ce: null,
            fe: null,
            time: null
        }
    },
    methods: {
        sendMessage() {
            let url = "https://api.weatherapi.com/v1/current.json?key=5579bd6431aa4b8583895926232907&q="
            let country = this.$refs.name.value
            fetch(url + country)
            .then((response) => {
                return response.json()
            })
            .then((data) => {
                this.image = 'https:' + data.current.condition.icon
                this.country = data.location.name + ', ' + data.location.country
                this.small = data.location.tz_id
                let time = data.location.localtime.split(' ')[1]
                this.time = time
                this.ce = data.current.temp_c
                this.fe = data.current.temp_f
                this.$refs.enable.classList.remove('disable')
            })
            setInterval(() => {
                let url = "https://api.weatherapi.com/v1/current.json?key=5579bd6431aa4b8583895926232907&q="
                let country = this.$refs.name.value
                fetch(url + country)
                .then((response) => {
                    return response.json()
                })
                .then((data) => {
                    this.image = 'https:' + data.current.condition.icon
                    this.country = data.location.name + ', ' + data.location.country
                    this.small = data.location.tz_id
                    let time = data.location.localtime.split(' ')[1]
                    this.time = time
                    this.ce = data.current.temp_c
                    this.fe = data.current.temp_f
                })
            }, 10000)
        }
    }
}
</script>
<style>
h1 {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}
.disable{
    display: none;
}
</style>
