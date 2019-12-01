<template>
    <el-container>
        <el-header id="header">
            <el-row justify="center" type="flex">
                <el-col :span=18>
                    <h1>贴吧签到 V2.0</h1>
                    <p>基于云端的自动签到工具，签到你的贴吧，解放你的双手</p>
                </el-col>
            </el-row>
        </el-header>

        <el-main id="main">
            <el-row :gutter="10" justify="center" type="flex">
                <el-col :offset=1 :span="4">
                    <i class="el-icon-user"></i> 总用户：{{ user_count }} 🔺
                </el-col>
                <el-col :span="4" justify="center">
                    <i class="el-icon-check"></i> 今日签到：{{ today_sign }} 🔺
                </el-col>
                <el-col :span="4" justify="center">
                    <i class="el-icon-position"></i> 累计签到：{{ total_sign }} 🔺
                </el-col>
            </el-row>

            <el-row :gutter="40" id="feature1" type="flex">
                <el-col :offset="10" :span="2" id="item1">永久免费</el-col>
            </el-row>

            <el-row :gutter="40" id="feature2" type="flex">
                <el-col :offset="11" :span="2" id="item2">方便快捷</el-col>
            </el-row>

            <el-row :gutter="40" id="feature3" type="flex">
                <el-col :offset="12" :span="2" id="item3">持久更新</el-col>
            </el-row>


            <el-row :gutter="40" id="feature4" type="flex">
                <el-col :offset="12" :span="2" id="item4">隐私保护</el-col>
            </el-row>

            <el-row :gutter="40" id="feature5" type="flex">
                <el-col :offset="11" :span="2" id="item5">一吧多签</el-col>
            </el-row>

            <el-row :gutter="40" id="feature6" type="flex">
                <el-col :offset="10" :span="2" id="item6">快乐交流♂</el-col>
            </el-row>

            <el-row justify="center" type="flex">
                <el-col :offset=11 :span=18>
                    <div id="start">
                        <el-button :round="true" @click="start" icon="el-icon-mouse" type="primary">开始签到</el-button>
                    </div>
                </el-col>
            </el-row>
        </el-main>

        <el-footer id="footer">
            <el-row>
                <p>Copyright ©2015-2019 All rights reserved</p>
                <p>由Django+Vue强力驱动</p>
            </el-row>
        </el-footer>

    </el-container>
</template>

<script>
    export default {
        name: "Index",
        data: function () {
            return {
                user_count: 999,
                today_sign: 999,
                total_sign: 999,
            }
        },
        mounted: function () {
            var self = this
            fetch("status/?t="+(Date.parse(new Date()) / 1000))
                .then(res => res.json())
                .then(function (res) {
                    self.user_count = res.user_count
                    self.today_sign = res.today_sign
                    self.total_sign = res.total_sign
                })
        },

        methods: {
            start: function () {
                let confirm = this.$confirm
                let notify = this.$notify
                fetch("image/?t="+(Date.parse(new Date()) / 1000))
                    .then(res => res.json())
                    .then(function (res) {
                        var storage = window.localStorage;
                        storage.setItem("sign", res.sign)
                        confirm('<img src=https://' + res.imgurl + '></img>', '使用贴吧客户端/百度网盘扫码', {
                            dangerouslyUseHTMLString: true,
                            distinguishCancelAndClose: true,
                            confirmButtonText: '扫码成功',
                            cancelButtonText: '放弃扫码',
                            center: true
                        }).then(() => {
                            let sign = storage.getItem("sign")
                            fetch("bduss/?sign=" + sign+"&t="+(Date.parse(new Date()) / 1000))
                                .then(function (res) {
                                    if (res.status === 200) {
                                        notify({
                                            message: '恭喜你，提交成功了呢，快刷新看看贴吧是不是已经开始签到了',
                                            type: 'success'
                                        })
                                    } else {
                                        notify({
                                            message: '你没有扫码瞎点击什么扫码成功呀~',
                                            type: 'error'
                                        })
                                    }
                                })
                        }).catch(() => {
                            notify.error("你放弃了扫码哦");
                        })
                    })
            }
        }
    }
</script>

<style scoped>
    h1 {
        text-align: center;
    }

    #item1 {
        background-color: red;
    }

    #item2 {
        background-color: #42b983;
    }

    #item3 {
        background-color: salmon;
    }

    #item4 {
        background-color: aqua;
    }

    #item5 {
        background-color: cornflowerblue;
    }

    #item6 {
        background-color: yellow;
    }

    #feature1 {
        margin-top: 30px;
    }

    #feature2 {
        margin-top: 5px;
    }

    #feature3 {
        margin-top: 5px;
    }

    #feature4 {
        margin-top: 5px;
    }

    #feature5 {
        margin-top: 5px;
    }

    #feature6 {
        margin-top: 5px;
    }

    #main {
        margin-top: 40px;
    }

    #footer {
        margin-top: 220px;
    }

    #start {
        margin-top: 50px;
    }

    p {
        text-align: center;
    }

</style>