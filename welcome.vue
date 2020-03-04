
<template>

<!--BASIC LINE FOR HANDLING HEADER -->
<div class="container below-header"><div id="base-views"> 

<div class="singleframe-outer welcomepage">
   
    <div class="singleframe-inner" style="max-width: none">
        <div class="generalcontainer centered">
            <div class="generalcontainer" style="width: 80%; left: 10%;">
                <h5 class="generalcontainer" style="font-family: 'Playfair Display', serif; font-weight: 500;">
                    Welcome to Epiloge
                </h5> 

                <h1 class="generalcontainer welcomeheading2">
                    <b>Build your network with your work and knowledge</b>
                </h1> 

                <h6 class="generalcontainer aboutusheadingsub-small margintop marginbottom">
                    Epiloge is a community for students and professionals to connect easily in their field of interest
                </h6>

                 <div class="generalcontainer" v-if="!isLoggedIn">
                    <router-link class="button join fullmobile" to="/signup" @click.native="deleteSignupData()">
                        <span v-if="!wasInvited">Sign up for free</span>
                        <span v-if="wasInvited">Join to accept your invitation</span></router-link>
                </div>
            </div>
        </div>

      
    </div>

    
</div>

<div class="singleframe-outer" style="padding-top: 30px; padding-bottom: 25px; ">
    
    <div class="singleframe-inner halfwindow margintop marginbottom">
       
          <h3 class="generalcontainer welcomeheading-small welcomeheading-notred">
            Share your papers, article or projects
        </h3> 

        <h6 class="generalcontainer aboutusheadingsub-small marginbottom ">
            Keep it brief or write more, you pick the topic
        </h6>

        <router-link  to="/explore" class="button" style="border-color: #ccc">Explore more</router-link>
       
        
    </div>
    
    <div class="singleframe-inner halfwindow rightcentric">
         <div class="generalcontainer nomobile">
            <div class="aboutus-picture welcome" id="blend-in-cover" v-bind:style="{ 'background-image': 'url(' + image4 + ')' }"></div>
        </div>       
    </div> 


</div>

<div class="singleframe-outer aboutus-text" style="padding-bottom: 0px;" ref="example">
    
    <div class="singleframe-inner marginbottom" style="max-width: none;">
        <div class="heading-container">
            <span class="headertext welcomelarge">Curated articles & projects</span>  
            <div class="headerbuttons nomobile" style="bottom: 6px; margin-right: 5px;">
                <router-link to="/explore" class="movetoright smalltext">   
                    <div class="movetoleft">See more</div>
                    <div class="movetoleft" style="margin-top: 5px;">
                        <icon-base width="16" height="16" iconColor="rgba(0,0,0,0.65)" icon-name="next" class="movetoleft">
                            <icon-next/>
                        </icon-base>
                    </div>
                </router-link>
            </div> 
        </div>

        <loading-container-small v-if="exampleProcessing"></loading-container-small>

         <project-explore-detail
            v-if="shownExamples.length"
            :items="shownExamples"  
            :entityData="{}"
            :whyuseepiloge="true">
        </project-explore-detail>

        <notice-container-text v-if="!exampleProcessing && queriedAPI && !shownExamples.length">
            We had problems loading the examples, please refresh this page or try later
        </notice-container-text>
        
    </div> 


</div>

<div class="singleframe-outer bordertop" style="padding-top: 40px; padding-bottom: 10px; ">
    
    <div class="singleframe-inner halfwindow marginbottom">
         <h3 class="generalcontainer welcomeheading-small welcomeheading-notred">
            Get to know people in your field
        </h3> 
        
        <h6 class="generalcontainer aboutusheadingsub-small marginbottom">
            Connect and follow students, colleagues and other people in your field of interest
        </h6>
       
        <router-link  to="/whyuseepiloge" class="button" style="border-color: #ccc">Why join Epiloge?</router-link>
        
    </div> 

    <div class="singleframe-inner halfwindow rightcentric">
        <div class="generalcontainer nomobile">
            <div class="aboutus-picture welcome" id="blend-in-cover" v-bind:style="{ 'background-image': 'url(' + image3 + ')' }"></div>
        </div>
    </div>
    
</div>

<div class="singleframe-outer aboutus-text" style="padding-bottom: 10px;" ref="users">
    
    <div class="singleframe-inner marginbottom" style="max-width: none;">
        <div class="heading-container">
            <span class="headertext welcomelarge">People on Epiloge</span>  
            <div class="headerbuttons" style="bottom: 6px; margin-right: 5px;" v-if="!queriedMoreUserAPI && !loadMoreUsersProcessing">
                <a @click="getMoreWelcomeUsers()" class="movetoright smalltext notunderlined">   
                    <div class="movetoleft">See more</div>
                    <div class="movetoleft" style="margin-top: 5px;">
                        <icon-base width="16" height="16" iconColor="rgba(0,0,0,0.65)" icon-name="next" class="movetoleft">
                            <icon-next/>
                        </icon-base>
                    </div>
                </a>
            </div> 
            <div class="headerbuttons" style="bottom: 6px; margin-right: 35px;" v-if="loadMoreUsersProcessing">
                <span class="buttonspinner"></span>
            </div>
            <div class="headerbuttons" style="bottom: 6px; margin-right: 5px;" v-if="queriedMoreUserAPI && !loadMoreUsersProcessing">
                <router-link to="/login" class="movetoright smalltext notunderlined">   
                    <div class="movetoleft">See more</div>
                    <div class="movetoleft" style="margin-top: 5px;">
                        <icon-base width="16" height="16" iconColor="rgba(0,0,0,0.65)" icon-name="next" class="movetoleft">
                            <icon-next/>
                        </icon-base>
                    </div>
                </router-link>
            </div> 
        </div>

        <loading-container-small v-if="usersProcessing"></loading-container-small>

        <div class="generalcontainer margintop" style="margin-top: 35px;">
            <suggestion-detail
                welcomepage
                showLocationBelow
                v-for="(user, index) in shownUsers"
                :user="user"
                :index="index"
                :key="user._id"
                type="">
            </suggestion-detail>
        </div>
        <loading-container-small v-if="loadMoreUsersProcessing"></loading-container-small>

        <notice-container-text v-if="!usersProcessing && queriedUserAPI && !shownUsers.length">
            We had problems showing curated people, please refresh  or try later
        </notice-container-text>
        
    </div> 


</div>


<div class="singleframe-outer" style="max-width: none;padding-top: 60px; padding-bottom: 60px;" v-if="!isLoggedIn">
    <div class="singleframe-inner centered" style="border-top: 1px solid lightgray; padding-top: 50px;">
        <h1 class="generalcontainer margintop marginbottom welcomeheading2-below">
            Sign up to create a profile, connect and write
        </h1>

         <div class="generalcontainer centered" >
            <router-link class="button join" to="/signup" @click.native="deleteSignupData()">Join now</router-link>
        </div>
    </div>
</div>


<link-footer></link-footer>
</div></div>

</template>

<script>
import _ from 'lodash'
import LinkFooter from './LinkFooter.vue'
import ProjectExploreDetail from '../projects/ProjectExploreDetail.vue'
import SuggestionDetail from '../invite/SuggestionDetail.vue'
export default {
    name: 'Welcome',    
     components: {      
        'link-footer' : LinkFooter,
        'project-explore-detail' : ProjectExploreDetail,
        'suggestion-detail' : SuggestionDetail
    },   
    data () {
        return { 
          
            image0: '',
            image1: '',
            image1Mobile: '',
            image2: '',
            image3: '',
            image4: '',
            wasInvited: false,

            aws_url: process.env.AWS_URL,

            exampleProcessing: false,
            usersProcessing: false,
            loadMoreUsersProcessing: false,
            shownExamples: [],
            shownUsers: [],
            shownUsersSkip: 0,
            queriedAPI: false,
            queriedUserAPI: false,
            queriedMoreUserAPI: false          
          
        }
    },     
    created: function(){    
        this.$_HelperFunctions_setTitleReverse('Build your network with your work and knowledge');  
        this.image1Mobile = this.aws_url + '/static/image-welcome6-mobile.jpg';
        this.image0 = this.aws_url + '/static/welcome-image34.jpg';        
        this.image3 = this.aws_url + '/static/screen-grow.jpg';
        this.image4 = this.aws_url + '/static/screen-writing.jpg';
       
        if (this.$route.params.invitationtoken && !this.isLoggedIn) {
          
            this.wasInvited = true;
            this.$store.commit('setInvitationToken', this.$route.params.invitationtoken);
        }
        this.setProfileCachePath();

        //for the stored projects/articles
        var storedExamples = this.$store.state.whyUseEpilogeExamples;      
        this.queriedAPI = false;
        if (storedExamples.length) {
            this.shownExamples = storedExamples;
            this.queriedAPI = true;
        } else {
            window.scrollTo(0,0); //needed for reloading page
        }  

        //for the stored user examples
        var storedUsers = this.$store.state.userExamples;
        this.queriedUserAPI = false;
        if (storedUsers.length) {
            this.shownUsers = storedUsers;
            this.queriedUserAPI = true;
            if (this.shownUsers.length > 4) this.queriedMoreUserAPI = true;
        }

    },
    computed: {
        isLoggedIn(){ return this.$store.state.isLoggedIn }
    },
    mounted() {
        this.$nextTick(function() {
            window.addEventListener('scroll', this.onScroll);
            this.onScroll(); // needed for initial loading on page
        });        
    },
    beforeDestroy() {
        window.removeEventListener('scroll', this.onScroll);
    },
    methods: {  
      
        setProfileCachePath() {
            this.$store.commit('setProfileCachePath', this.$route.path);
        }, 
        goToEmailSignup() {
            this.$router.push('/signup');
        },
        deleteSignupData() {
            this.$store.commit('setSignupData', {});
        },
        getExamplesWriting() {

            //we only want to lead the projects once
            if (!this.queriedAPI) {
            
                this.exampleProcessing = true;
                
                var windowWidth = window.innerWidth;
                var numberOfExamples = 3;
                if (windowWidth > 990) {
                    numberOfExamples = 3; //desktop
                } else if (windowWidth > 600) {
                    numberOfExamples = 2; //tablet
                } else {
                    numberOfExamples = 3; //mobile
                }

                this.queriedAPI = true;
                
                let uri = process.env.API_URL + 'welcomewritingexamples/' + numberOfExamples;         
                this.axios.get(uri).then((response) => {
              

                    var data = response.data;   
                   
                    this.exampleProcessing = false; 
                 
                    if (data.success) {
                        //for data processing reasons, let's create a fake 'connections' array
                        var connections = [];
                        for (var i=0; i < data.projects.length; i++) {
                            connections.push({projectID: data.projects[i]._id});
                        }
                        this.$_HelperFunctions_setProjectsSingleOrg(connections, data.projects);  
                        this.shownExamples = this.$_HelperFunctions_sortProjects(connections);   

                        this.$store.commit('setWhyUseEpilogeExamples', this.shownExamples); 
                    } else {
                        //don't do anything

                    }

                });;
            }
        },
        getUsersWriting() {

            //we only want to load the projects once
            if (!this.queriedUserAPI) {
            
                this.usersProcessing = true;
                
                var windowWidth = window.innerWidth;
                var numberOfExamples = 4;
                if (windowWidth > 1021) {
                    numberOfExamples = 4; //desktop
                } else if (windowWidth > 600) {
                    numberOfExamples = 3; //tablet
                } else {
                    numberOfExamples = 4; //mobile
                }

                this.queriedUserAPI = true;
                
                let uri = process.env.API_URL + 'welcomeuserexamples/' + numberOfExamples;         
                this.axios.post(uri).then((response) => {              

                    var data = response.data;   
                   
                    this.usersProcessing = false; 
                 
                    if (data.success) {
                         
                        this.shownUsers = data.users;       
                        this.shownUsersSkip = data.skipValue + numberOfExamples;             

                        this.$store.commit('setUserExamples', this.shownUsers); 
                    } else {
                        //don't do anything

                    }

                });;
            }
        },
        getMoreWelcomeUsers() {
           
            if (!this.queriedMoreUserAPI) {
            
                this.loadMoreUsersProcessing = true;
                
                var numberOfExamples = 12;                

                this.queriedMoreUserAPI = true;
                
                let uri = process.env.API_URL + 'welcomeuserexamples/' + numberOfExamples;         
                this.axios.post(uri, {skipValue: this.shownUsersSkip}).then((response) => {              

                    var data = response.data;   
                   
                    this.loadMoreUsersProcessing = false; 
                 
                    if (data.success) {
                         
                        //here need to make this unique!!!
                        this.shownUsers = this.shownUsers.concat(data.users);                    

                        this.$store.commit('setUserExamples', this.shownUsers); 
                    } else {
                        //don't do anything

                    }

                });;
            }
        },
        
        onScroll() {
         
            var exampleHeading = this.$refs["example"];
            var usersHeading = this.$refs["users"];
            
            if (exampleHeading) {
                var marginTopExample = exampleHeading.getBoundingClientRect().top;
                var innerHeight = window.innerHeight;
            
                if ((marginTopExample - innerHeight) < -50) {                 
                    this.getExamplesWriting();
                }                               
            }  
            if (usersHeading) {
                var marginTopUsers = usersHeading.getBoundingClientRect().top;
                var innerHeight = window.innerHeight;
             
                if ((marginTopUsers - innerHeight) < -50) {                 
                    this.getUsersWriting();
                }                               
            }  
        }  
        
        
    }
}
</script>
