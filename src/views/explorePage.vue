<template>
    <div class="explore">
        <div class="cultivate" v-if="$store.monster.name">
            你遇到了
            <span class="el-tag el-tag--danger" @click="openMonsterInfo" v-text="monster.name" />
            <div class="storyText">
                <div class="storyText-box">
                    <el-scrollbar ref="scrollbar" always>
                        <p class="fighting" v-if="isFighting" v-text="`${guashaRounds}回合 / 10回合`" />
                        <p v-for="(item, index) in texts" :key="index" v-html="item" @click="openEquipmentInfo(openEquipItemInfo)" />
                    </el-scrollbar>
                </div>
            </div>
            <div class="actions">
                <el-button
                    v-for="(action, index) in actionButtons"
                    :key="index"
                    class="item"
                    @click="operate(action.name)"
                    :disabled="action.disabled"
                >
                    {{ action.text }}
                    <span class="shortcutKeys">{{ action.shortcut }}</span>
                </el-button>
            </div>
        </div>
        <div class="cultivate error" v-else>
            <el-result icon="error" title="缺少对战信息" sub-title="请返回地图重新探索">
                <template #extra>
                    <el-button :type="!player.dark ? 'primary' : ''" @click="$router.push('/map')">返回地图</el-button>
                </template>
            </el-result>
        </div>
    </div>
</template>

<script>
import equip from '@/plugins/equip';
import achievement from '@/plugins/achievement';

export default {
    data() {
        return {
            texts: [],
            isEnd: false,
            player: {},
            monster: {},
            loading: true,
            victory: false,
            timerIds: [],
            isFighting: false,
            guashaRounds: 10,
            isFailedRetreat: false,
            isCaptureFailed: false,
            openEquipItemInfo: {}
        };
    },
    computed: {
        actionButtons() {
            return [
                {
                    name: 'startFight',
                    text: '发起战斗',
                    shortcut: '(Q)',
                    disabled: this.isEnd
                },
                {
                    name: 'harvestPet',
                    text: '收服对方',
                    shortcut: '(E)',
                    disabled: this.isCaptureFailed
                },
                {
                    name: 'runAway',
                    text: '立马撤退',
                    shortcut: '(R)',
                    disabled: this.isFailedRetreat
                },
                {
                    name: 'explore',
                    text: '继续探索',
                    shortcut: '(F)',
                    disabled: this.player.health <= 0 || !this.isEnd
                },
                {
                    name: 'goHome',
                    text: '回家疗伤',
                    shortcut: '(G)',
                    disabled: !this.isEnd
                }
            ];
        }
    },
    beforeUnmount() {
        this.stopFight();
        window.removeEventListener('keydown', this.handleKeyDown);
    },
    mounted() {
        this.player = this.$store.player;
        if (this.$store.monster.name) {
            this.loading = false;
            this.monster = this.$store.monster;
        }
        window.addEventListener('keydown', this.handleKeyDown);
    },
    methods: {
        handleKeyDown(event) {
            this.operate(event.key.toLowerCase());
        },
        operate(action) {
            switch (action) {
                case 'q':
                case 'startFight':
                    this.startFight();
                    break;
                case 'e':
                case 'harvestPet':
                    this.harvestPet();
                    break;
                case 'r':
                case 'runAway':
                    this.runAway();
                    break;
                case 'f':
                case 'explore':
                    this.explore();
                    break;
                case 'g':
                case 'goHome':
                    this.goHome();
                    break;
                default:
                    break;
            }
        },
        openMonsterInfo() {
            // 打开野怪详细信息弹窗
        },
        startFight() {
            // 开始战斗逻辑
        },
        harvestPet() {
            // 收服逻辑
        },
        runAway() {
            // 撤退逻辑
        },
        explore() {
            // 探索逻辑
            this.$router.push('/map');
        },
        goHome() {
            // 回家逻辑
            this.$router.push('/home');
        },
        stopFight() {
            this.timerIds.forEach(id => clearInterval(id));
            this.timerIds = [];
        }
    }
};
</script>

<style scoped>
.explore {
    display: flex;
    flex-direction: column;
    align-items: center;
}
.actions {
    display: flex;
    justify-content: space-around;
    margin-top: 20px;
}
.item {
    margin: 5px;
}
.storyText-box {
    max-height: 300px;
    overflow-y: auto;
}
</style>
