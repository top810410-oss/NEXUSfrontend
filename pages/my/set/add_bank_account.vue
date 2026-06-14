<template>
    <view class="page">
        <view class="content">
            <form @submit="addBank" @reset="">
            <view class="uni-form-item">
                <text class="title"><text style="margin-right: 10px;">*</text>姓名</text>
                <input class="uni-input" @input="name"  type="text" placeholder="请输入姓名"/>
            </view>
            <view class="uni-form-item">
                <text class="title"><text style="margin-right: 10px;">*</text>银行名称</text>
                <input class="uni-input" @input="bankname" type="bankname" placeholder="请输入银行名称"/>
            </view>
            <view class="uni-form-item">
                <text class="title"><text style="margin-right: 10px;">*</text>银行卡账号</text>
                <input class="uni-input"  @input="account" type="text" placeholder="请输入银行卡账号"/>
            </view>
            <button class="withdraw" form-type="submit" type="submit">绑定</button>
            </form>
        </view>
    </view>
</template>


<script>
    import _get from '../../../common/_get';
    import _hook from '../../../common/_hook';
    export default {
        data() {
            return {
                requestParams:{
                    'name':'',
                    'bankname':'',
                    'account':'',
                    'bank_type':3
                }
            }
        },
        onShow(){
            _hook.routeSonHook();
        },
        methods: {
            name(e){
                return this.requestParams.name = e.detail.value.trim();
            },
            bankname(e){
                return this.requestParams.bankname = e.detail.value.trim();
            },
            account(e){
                return this.requestParams.account = e.detail.value.trim();
            },
            bank_type(e){
                return this.requestParams.bank_type = e.detail.value.trim();
            },
            addBank(e){
				if(this.requestParams.name == ''){
				    return this.showToast('请输入姓名!');
				}
				if(this.requestParams.bankname == ''){
				    return this.showToast('请输入银行名称!');
				}
                if(this.requestParams.account == ''){
                    return this.showToast('请输入银行卡号!');
                }
                
                
                _get.addUserBank(this.requestParams,function (res) {
                    uni.navigateBack();
                });
            },
            showToast(msg){
                return uni.showToast({title:msg,icon:'none'});

            }
        }
    }
</script>

<style>
    page{
        background-color: white;
    }
    .page{
        border-top: 1px solid #e3e3e3;
    }
.uni-input{
    border: none;
    text-align: right;
}
    .uni-form-item{
        border-bottom: 1px solid #e3e3e3;
    }
    .content .withdraw{
        margin-top: 70upx;
        width: 90%;
        background-color: #5693ee;
        color: white;
        height: 70upx;
        line-height:  70upx !important;
        border-radius: 40upx;
        font-size: 36upx;
        cursor: pointer;
    }
</style>
