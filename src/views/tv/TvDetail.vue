<template>
    <div>
        电视剧{{titl}}-详情页面
    </div>
</template>
<script>
export default {
    data(){
        return{
            title:""
        }
    },
    created() {
        let thisId=this.$router.params.id
        this.getDate(thisId);
    },
    methods: {
        getDate(){
            let baseUrl = "https://bird.ioliu.cn/v2?url=";
            let requestUrl="https://m.douban.com/rexxar/api/v2/tv/"+id
        axios.get(baseUrl+requestUrl).then((res)=>{
            console.log(res);
            this.title=res.data.title;
            
            

        })    
        }
    },
}
</script>