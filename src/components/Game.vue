<template>
  <div id="plano"></div>
</template>
<style>
  #plano {
    position:absolute;
    top:0;
    left:0;
    width:1280px;
    height:640px;
    margin: 0 auto;
  }

  #plano canvas {
    display: block;
    margin: 0 auto;
  }
</style>
<script>
    /* eslint-disable */
    /* eslint-disable no-unused-vars */
    import 'pixi'
    import 'p2'
    import Phaser from 'phaser'
    /* eslint-enable no-unused-vars */

    export default{
        name: 'phaser',
        data: () => ({
            game: null,
            sprites: {},
        }),
        mounted () {
            this.$on('click', function(){
                console.log('click')
            })
            let self = this
            if (this.game == null) {
                this.game = new Phaser.Game({
                    width: 1280,
                    height: 640,
                    renderer: Phaser.AUTO,
                    antialias: true,
                    parent: this.$el,
                    state: {
                        preload: function preload () {
                            self.preload(this)
                        },
                        create: function create () {
                            self.create(this)
                        },
                        update: function update () {
                            self.update(this)
                        },
                        render: function render () {
                            self.render(this)
                        }
                    }
                })
            }
        },
        methods: {
            preload () {
                this.game.load.image('block', './static/assets/block.png')
            },
            create (phaser) {
                let self = this;
                var sprite = this.game.add.sprite(this.game.world.centerX, this.game.world.centerY, 'block')
                sprite.anchor.set(0.5)
                sprite.inputEnabled = true
                sprite.input.enableDrag()
                sprite.events.onInputDown.add(function() {
                    console.log('sprite down')
                })
            },
            update (phaser) {
            },
            render() {
                var debug
                debug = this.game.debug
                debug.inputInfo(32, 32);
                debug.pointer( this.game.input.activePointer );
                debug.spriteInputInfo(this.game.world.children[0], 32, 150)
            }
        },
        destroyed () {
            // this.game.destroy()
        }
    }
</script>