<template>
    <div>
        <h3>Documents</h3>
        <div class="divcontainer">
            <label for="title">Title: </label>
            <input type="text" name="title" id="title" v-model="title" style="width: 100%"/>
        </div>
        <div class="divcontainer">
            <label for="subject">Subject: </label>
            <input type="text" name="subject" id="subject" v-model="subject" style="width: 100%"/>
        </div>
        <div class="divcontainer">
            <label for="contents">contents: </label>
            <textarea id="contents" name="contents" placeholder="Contents" v-model="contents" style="width:100%; height:100px"></textarea>
        </div>
        <div class="divcontainer">
            <button @click="sendMsg()">Send Server</button>
            <button @click="getMsg()">Get Button</button>
            <button @click="sendEmail()">Send Email</button>
        </div>
    </div>
</template>

<script>
import { reactive, toRefs, onUpdated } from 'vue'

export default {
    setup () {
        const state = reactive({
            count: 0,
            host: "http://ec2-13-125-55-53.ap-northeast-2.compute.amazonaws.com:7771",
            // host: "http://localhost:7771",
            title: '',
            subject: '',
            contents: '',
        })

        // onUpdated (() => {
        //     console.log(state)
        // })

        const sendEmail = () => {
            // const subject = document.getElementById("subject").value;
            // const title = document.getElementById("title").value;
            // const contents = document.getElementById("code").value;
            const subject = state.subject;
            const title = state.title;
            const contents = state.contents;

            const requestData = {
                title: title,
                subject: subject,
                contents: contents,
            }

            const url = state.host + "/email";
            fetch(url, {
                method: "POST",
                headers: {
                    'Content-Type': 'application/json',
                },
                body: JSON.stringify(requestData)
            }).then(response => response.json())
            .then(data => console.log(data))
            .catch(error => {
                console.error('에러: ', error)
            })
        }

        const sendMsg = () => {
            
            const url = state.host + "/sendmsg"

            // const subject = document.getElementById("subject").value;
            // const title = document.getElementById("title").value;
            // const contents = document.getElementById("code").value;
            const subject = state.subject;
            const title = state.title;
            const contents = state.contents;

            const requestData = {
                title: title,
                subject: subject,
                contents: contents,
            }

            fetch(url, {
                method: "POST",
                headers: {
                    'Content-Type': 'application/json',
                },
                body: JSON.stringify(requestData) 
            }).then(response => response.json())
            .then(data => console.log(data))
            .catch(error => {
                console.error('에러: ', error)
            })
        }

        const getMsg = () => {
            const url = state.host + "/getmsg"
            
            fetch(url, {
                method: "GET"
            }).then((response) => {
                return response.json();
            }).then(data => {
                console.log(data);
            }).catch(error => {
                console.error('에러: ', error)
            })
        }
    
        return {
            ...toRefs(state),
            sendEmail,
            sendMsg,
            getMsg,
            onUpdated
        }
    }
}
</script>

<style lang="scss" scoped>

</style>
