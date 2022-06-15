// APP APP APP APP APP APP APP APP APP APP APP APP APP APP APP APP APP APP APP

<div class="head">
      
    <div class="head-project">
    <div>
        <img width="35px" src=".\assets\icon_marker02.svg">
    </div>
    <div>
        ITALY
    </div>
    </div>  
    <div>
    <img width="120" src="./assets/trepees-logo.png">
    </div>  
    <div 
    class="head-nav"
    @click.stop="shouldOpenNav = true"
    >
    <div 
        v-show="shouldOpenNav"
        class="head-nav-items"
    >
        <div 
        class="head-nav-items-close"
        @click.stop="shouldOpenNav = false"
        >
        X
        </div>
        <br>
        <router-link to="/">Home</router-link>
        <router-link to="/new">New Project</router-link>
    </div>
    <img height="30px" src=".\assets\icon_hamburger.svg">
    </div>  
    
</div>

 .container {
    width: 100%;
    height: 100%;
    // width: 360px;
    // height: 600px;
    color: white;
    background-color: #2d2d2f;
  }

  .head {
    padding-top: 20px;
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    // background-color: gray;

    &-project {
      // width: 100px;
      // background-color: red;
      display: flex;
      flex-direction: column;
      // justify-content: center;
      align-items: center;


      padding: 0px 0 0 20px;
      font-size: 12px;
      font-weight: bold;
      color: darkgray;
    }

    &-logo {
      width: auto;
    }

    &-nav {
      // background-color: red;
      // width: 100px;
      display: flex;
      flex-direction: column;
      padding: 5px 20px 0 0;
      font-size: 14px;
      font-weight: bold;
      line-height: 20px;
      color: white;

      &-items {
        position: absolute;
        top: 8px;
        right: 8px;
        width: 100px;
        height: 100px;

        display: flex;
        flex-flow: column;
        justify-content: space-around;

        padding: 10px;
        background-color: white;
        color: gray;

        &-close {
          // width: 20px;
          // height: 20px;
          // background-color: crimson;

          align-self: flex-end;
        }
      }

      a {
        text-decoration: none;
        color: gray;

        &:hover {
          background-color: cornsilk;
        }

        &.router-link-exact-active {
          color: red;
        }
      }
    }

}
