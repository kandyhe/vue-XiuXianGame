methods: {
    startFight() {
        if (this.isEnd) return;

        this.isEnd = true;
        this.victory = false;

        // 自动战斗逻辑
        const fightLoop = setInterval(() => {
            if (this.player.health <= 0 || this.monster.health <= 0) {
                clearInterval(fightLoop);
                this.isEnd = true;

                if (this.monster.health <= 0) {
                    this.handleVictory(); // 处理胜利
                } else if (this.player.health <= 0) {
                    this.handleDefeat(); // 处理失败
                }
            } else {
                this.fightMonster(); // 自动执行一轮战斗
            }
        }, 100); // 每100ms进行一轮战斗
    },
    fightMonster() {
        // 省略具体战斗日志，仅计算战斗结果
        const monsterHarm = Math.max(1, this.monster.attack - this.player.defense);
        const playerHarm = Math.max(1, this.player.attack - this.monster.defense);

        this.player.health = Math.max(0, this.player.health - monsterHarm);
        this.monster.health = Math.max(0, this.monster.health - playerHarm);
    },
    handleVictory() {
        this.texts = [`你击败了${this.monster.name}，继续探索或回家疗伤！`];
        this.findTreasure(); // 调用奖励逻辑
    },
    handleDefeat() {
        this.texts = ['你被击败了，请回家疗伤后再战！'];
    },
    findTreasure() {
        // 省略具体道具日志，仅提示获得奖励
        this.texts.push('你获得了丰厚的奖励！');
        // 奖励逻辑...
    },
    goHome() {
        this.$router.push('/home'); // 简化回家逻辑
    },
}
