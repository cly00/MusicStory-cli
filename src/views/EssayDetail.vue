<template>
  <div class="container">
    <div class="row">
      <div class="main">
        <section class="bgw art">
          <h1 class="title" v-text="essayDetail.title"></h1>
          <div class="essay-us">
            <div class="us-box">
              <router-link class="avatar" to="/u/123">
                <el-avatar :size="48" :src="essayDetail.user.avatar"></el-avatar>
              </router-link>
              <div style="margin-left: 8px;">
                <div class="us-info">
                  <span class="us-name">
                    <router-link to="#">{{essayDetail.user.username}}</router-link>
                  </span>
                  <el-button class="btn btn-info" round size="mini">关注</el-button>
                </div>
                <div style="display: flex;">
                  <span class="ed">发表时间：{{essayDetail.createdTime}}</span>
                  <span class="ed">阅读：{{essayDetail.readNum}}</span>
                </div>
              </div>
            </div>
          </div>
          <article v-html="essayDetail.content"></article>
        </section>
        <div id="page-comment">
          <section class="pg-comment">
            <div style="display:flex;">
              <el-avatar :size="40" :src="this.$parent.avatar"></el-avatar>
              <div class="comment-form">
                <el-form>
                  <el-form-item>
                    <el-input type="textarea" placeholder="写下你的评论..."></el-input>
                  </el-form-item>
                  <el-form-item>
                    <div class="cm-s">
                      <div>
                        <span>Enter 发表</span>
                      </div>
                      <div>
                        <el-button class="btn btn-info" round size="mini">发布</el-button>
                        <el-button class="btn btn-info" round size="mini">取消</el-button>
                      </div>
                    </div>
                  </el-form-item>
                </el-form>
              </div>
            </div>
            <h3 class="cmt">
              <div class="cmt-info">
                <span class="cmt-all">全部评论</span>
                <span class="cmt-num">{{comments.content.length}}</span>
              </div>
            </h3>
            <div class="comt">
              <div style="display: flex;" v-for="(item, index) in comments.content" :key="index">
                <router-link class="avatar" to="/#">
                  <el-avatar :size="40" :src="item.avatar"></el-avatar>
                </router-link>
                <div class="comt-content">
                  <div class="comt-uname">
                    <router-link to="#">{{item.username}}</router-link>
                  </div>
                  <div class="comt-flr">
                    <span>{{index+1}} 楼</span>
                    <span v-text="item.createdTime"></span>
                  </div>
                  <div class="comt-info">{{item.content}}</div>
                  <div class="comt-other">
                    <span style="cursor: pointer;color: #b0b0b0;">
                      <i class="fa fa-comment" aria-hidden="true"> 回复</i>
                    </span>
                  </div>
                </div>
              </div>
            </div>
          </section>
        </div>
      </div>
      <aside style="width: 260px;">
        <section class="bgw extral">待填充</section>
      </aside>
    </div>
    <aplayer v-if="essayDetail.musicList.length>0"
    :audio="essayDetail.musicList" 
    :lrcType="3" 
    :autoplay="true"
    fixed />
  </div>
</template>

<script>
import Vue from 'vue';
import api from "@/api/api";
import APlayer from '@moefe/vue-aplayer';

Vue.use(APlayer);
export default {
  name: "essayDetail",
  props: {
    id: String
  },
  data() {
    return {
      essayDetail: {
        title: "",
        content: "",
        readNum: "",
        createTime: "",
        updateTime: "",
        user: {
          username: "",
          avatar: ""
        },
        musicList: []
      },
      comments: {
        content: []
      }
    };
  },
  mounted() {
    api.essays("", "/" + this.id).then(response => {
      this.essayDetail = response.data;
    });
    api.comments("", "/" + this.id + "/1").then(response => {
      this.comments = response.data;
    });
  },
  beforeDestroy() {
    api.readEssay("","/"+this.id);
  }
};
</script>>

<style scoped>
.container {
  background-color: #f9f9f9;
}
.row {
  box-sizing: initial;
  width: 1000px;
  padding-left: 16px;
  padding-right: 16px;
  margin-left: auto;
  margin-right: auto;
  display: flex;
  justify-content: center;
  align-items: flex-start;
  min-height: calc(100vh - 66px);
  padding-top: 10px;
  font-size: 16px;
}
*,
:after,
:before {
  box-sizing: border-box;
}
.main {
  flex-shrink: 0;
  width: 730px;
  margin-bottom: 24px;
  margin-right: 10px;
}
.bgw {
  background-color: #fff;
}
.art {
  border-radius: 4px;
  margin-bottom: 10px;
  padding: 24px;
}
.title {
  font-size: 30px;
  font-weight: 700;
  word-break: break-word;
  margin-bottom: 0.5em;
}
.essay-us {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 32px;
  font-size: 13px;
}
.us-box {
  display: flex;
  align-items: center;
}
.us-info {
  display: flex;
  align-items: center;
  margin-bottom: 6px;
}
.us-name {
  font-size: 16px;
  font-weight: 500;
  margin-right: 8px;
}
.ed {
  margin-right: 10px;
  color: #969696;
  font-size: 13px;
}
.pg-comment {
  padding: 24px;
  background-color: #fff;
  border-radius: 4px;
  margin-bottom: 10px;
}
.comment-form {
  margin-left: 10px;
  margin-bottom: 48px;
  flex-grow: 1;
}
.cmt {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 16px;
  padding-left: 12px;
  border-left: 4px solid #ec7259;
  font-size: 18px;
  font-weight: 500;
  height: 20px;
  line-height: 20px;
}
.cmt-info {
  display: flex;
  align-items: center;
}
.cmt-all {
  font-size: 18px;
  font-weight: 500;
  line-height: 20px;
}
.cmt-num {
  margin-left: 6px;
  font-size: 14px;
  font-weight: normal;
}
.cm-s {
  display: flex;
  justify-content: space-between;
  font-size: 14px;
  color: #969696;
  align-items: center;
}
.comt {
  margin-top: 30px;
  margin-bottom: 30px;
  line-height: 1.5;
}
.comt-content {
  flex-grow: 1;
  margin-left: 10px;
  margin-bottom: 20px;
  padding-bottom: 16px;
  border-bottom: 1px solid #eee;
}
.comt-uname {
  display: flex;
  align-items: center;
  font-size: 15px;
  font-weight: 500;
}
.comt-flr {
  margin-top: 2px;
  font-size: 12px;
  color: #969696;
}
.comt-info {
  margin-top: 10px;
  font-size: 16px;
  word-break: break-word;
}
.comt-other {
  position: relative;
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: 12px;
  font-size: 15px;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}
.extral {
  padding: 16px;
  border-radius: 4px;
  margin-bottom: 10px;
}
</style>>