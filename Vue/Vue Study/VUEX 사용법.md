# VUEX 사용법

```jsx
export default new Vuex.Store({
    state: {
        // data의 집합(중앙관리할 모든 데이터 === 상태)

        // this.$store.state 로 접근이 가능하다
        // 혹은 mapState를 통해 data에 매핑할 수 있다
        import { mapState } from 'vuex'
        ...mapState(['data1', 'data2'])
    },
    getters: {
        // state를 (가공해서) 가져올 함수들
        // computed와 같은 역할을 한다
        // 공식문서에서는 getters에서의 화살표 함수 사용을 권장한다

        // template에서  $store.getters.함수이름 으로 접근 가능하다
        // component의 computed에 매핑할 수 있다
        import { mapGetters } from 'vuex'
        ...mapGetters(['function1', 'function2'])

    },
    mutations: {
        // state를 변경하는 함수들이 존재하는 곳
        // mutations에 작성되지 않은 state변경 코드는 모두 동작하지 않는다
        // 모든 mutations 함수들은 동기적으로 동작하는 코드
        // 모든 함수들은 첫번째 인자로 state를 받아야 한다
        
        // commit을 통해 실행한다
        // 혹은 mapMutations를 통해 methods에 매핑할 수 있다
        import { mapMutations } from 'vuex'
        ...mapMutations(['function3', 'function4'])
    },
    actions: {
        // 범용적인 함수들. mutations에 정의한 함수를 actions에서 실행할 수 있다
        // 비동기 로직은 actions에서 정의해야 한다
        // actions의 첫번째 인자는 기본적으로 context가 된다
        // context에는 commit, dispatch, getters, rootGetters, rootState, state 등 모든 것이 들어있다
        // 일부만 사용할 경우 {} 를 통해 정해준다
        actionsFunction({ state, commit }, param) {
            console.log(state, commit, param)
        } 

        // dispatch를 통해 실행한다
        // 혹은 mapActions를 통해 methods에 매핑할 수 있다
        import { mapActions } from 'vuex'
        ...mapActions(['function3', 'function4'])

    },
    modules: {
        // vuex의 크기가 커져서 저장소를 모듈로 나눌 수 있다
        // 각 모듈은 자체적으로 state, getters, mutations, actions를 가질 수 있다
        // 모듈안에 또다른 모듈을 받을 수 있다
        
        //  namespace:true 옵션을 이용하여 해당 모듈의 getters, mutations, actions는 
        // 자동으로 등록된 모듈의 경로를 기반으로 네임스페이스가 지정된다.

        // 네임스페이스 모듈에서 전역 액션을 등록하려면 root:true 를 표시하고 handler 함수에 액션을 정의할 수 있다
    }
})
```