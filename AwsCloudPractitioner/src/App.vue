<script>
  import stepper from "./components/StepperComponent.vue";
  import quiz from "./components/QuizComponent.vue";
  export default {
    components: {
      stepper,
      quiz
    },
    
    data:()=>({
        questions: [
          {
          id:0,
          question: "Why is AWS more economical than traditional data centers for applications with varying compute workloads?",
          choices: [{answer:"Amazon EC2 costs are billed on a monthly basis",
          isCorrect:false},
          {answer:"Amazon EC2 costs are billed on a yearly basis",isCorrect:false},
          {answer:"Amazon EC2 instances can be launched on demand when needed.", isCorrect:true},
          {answer:"Users can permanently run enough instances to handle peak workloads.", isCorrect:false}],
          description: "The ability to launch instances on demand when needed allows users to launch and terminate instances in response to a varying workload.This is a more economical practice than purchasing enough on-premises servers to handle the peak load."
          },
        {
            id:1,
            question: "Which AWS service would simplify the migration of a database to AWS?",
            choices:[{answer: "AWS Storage Gateway", isCorrect:false},
            {answer:"AWS Database Migration Service (AWS DMS)", isCorrect:true},
            {answer:"Amazon EC2", isCorrect:false},
            {answer:"Amazon AppStream 2.0", isCorrect:false}],
            description : "AWS DMS helps users migrate databases to AWS quickly and securely. The source database remainsfully operational during the migration, minimizing downtime to applications that rely on the database. AWS DMS can migrate data to and from most widely used commercial and open-source databases."
        },
        {
            id:2,
            question:"Which AWS offering enables users to find, buy, and immediately start using software solutions in their AWS environment?",
            choices: [{answer:"AWS Config", isCorrect:false}, 
            {answer:"AWS OpsWorks", isCorrect:false}, {answer:"AWS SDK",isCorrect:false}, {answer:"AWS Marketplace", isCorrect:true}],
            description: "AWS Marketplace is a digital catalog with thousands of software listings from independent software vendors that makes it easy to find, test, buy, and deploy software that runs on AWS."
        },
        {
            id:3,
            question:"Which AWS networking service enables a company to create a virtual network within AWS?",
            choices: [{answer:"AWS Config", isCorrect:false}, {answer:"Amazon Route 53", isCorrect:false},
            {answer:"AWS Direct Connect",isCorrect:false},
            {answer:"Amazon Virtual Private Cloud (Amazon VPC)", isCorrect:true}],
            description: "Amazon VPC lets users provision a logically isolated section of the AWS Cloud where users can launch AWS resources in a virtual network that they define."
        },
    ],
    showQuiz: false,
  })
  }
</script>
<template>
  <body>
    <stepper @start-quiz="showQuiz = true"/>
    <div>
      <quiz v-if="showQuiz" :questions="questions"/>
    </div>
  </body>
</template>
<style>
body{
  background-image: linear-gradient(60deg,#ab3cfc 0%,#7e6df3 100%);
  color: #fff;
  width: 100%;
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>




