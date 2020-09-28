<template>
  
    <div>        
        
    </div>

</template>

<script>

export default {
    name: "RiskComparison",    
    data: () => ({
 
    }),
    methods: {  

    },
    created(){
    }, 
    filters: {
        truncate: function(value) {
            if(value != undefined){
                value = value.toString().substring(0, 8);
            }
            return value
        }
    }

};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>


