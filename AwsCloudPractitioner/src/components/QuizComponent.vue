<script>
    export default {
        props:{
            questions:Array,
            required: true
        },
        data:() =>({
            // questions: [
            //     {
            //         id:0,
            //         question: "Why is AWS more economical than traditional data centers for applications with varying compute workloads?",
            //         answers:["Amazon EC2 costs are billed on a monthly basis","Amazon EC2 costs are billed on a monthly basis","Users can permanently run enough instances to handle peak workloads.","Amazon EC2 instances can be launched on demand when needed."],
            //         wrong_answers: ["Amazon EC2 costs are billed on a monthly basis","Amazon EC2 costs are billed on a monthly basis","Users can permanently run enough instances to handle peak workloads."],
            //         correct_answer: "Amazon EC2 instances can be launched on demand when needed.",
            //         description: "The ability to launch instances on demand when needed allows users to launch and terminate instances in response to a varying workload.This is a more economical practice than purchasing enough on-premises servers to handle the peak load."
            //     },
            //     {
            //         id:1,
            //         question: "Which AWS service would simplify the migration of a database to AWS?",
            //         answers:["AWS Storage Gateway","Amazon EC2","Amazon AppStream 2.0","AWS Database Migration Service (AWS DMS)"],
            //         wrong_answers: ["AWS Storage Gateway","Amazon EC2","Amazon AppStream 2.0"],
            //         correct_answer: "AWS Database Migration Service (AWS DMS)",
            //         description : "AWS DMS helps users migrate databases to AWS quickly and securely. The source database remainsfully operational during the migration, minimizing downtime to applications that rely on the database. AWS DMS can migrate data to and from most widely used commercial and open-source databases."
            //     },
            //     {
            //         id:2,
            //         question:"Which AWS offering enables users to find, buy, and immediately start using software solutions in their AWS environment?",
            //         answers:["AWS Config","AWS OpsWorks","AWS SDK","AWS Marketplace"],
            //         wrong_answers: ["AWS Config","AWS OpsWorks","AWS SDK"],
            //         correct_answer: "AWS Marketplace",
            //         description: "AWS Marketplace is a digital catalog with thousands of software listings from independent software vendors that makes it easy to find, test, buy, and deploy software that runs on AWS."
            //         }
            // ],
            index: 0
        }),
        computed: {
            selectedQuestion() {
                return this.questions[this.index];
            },
        },
        methods:{
            nextItem() {
                if (this.index < this.questions.length - 1) {
                    this.index++;
                } else {
                    this.index = 0;
                }
            },
        //     displayQuestions(){
        //         let questions = data.results.map((question) => {
        //             for (let i = question.answers.length - 1; i > 0; i--) {
        //                 const j = Math.floor(Math.random() * (i + 1));
        //                 [question.answers[i], question.answers[j]] = [
        //                     question.answers[j],
        //                     question.answers[i],
        //                 ];
        //             }
        //     })
        // }
    }
}
</script>
<template>
    <div class="container">
        <div>
            <h2> {{ selectedQuestion.question }}</h2>
            <div>
                <button v-for="answer in selectedQuestion.answers" :key="answer">{{ answer }}</button>
            </div>

            <button @click="nextItem">Next</button>
        </div>
    </div>
</template>
<style></style>