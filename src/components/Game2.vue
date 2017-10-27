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
            text: '',
            sprites: {},
            color: null,
            colorRecord: null
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
                self.colorRecord = '#ffffff'
            }
        },
        methods: {
            preload () {
                this.game.load.image('block', './static/assets/block.png')
                this.game.load.image('grid', './static/assets/debug-grid-1920x1920.png')
                this.game.load.spritesheet('button', './static/assets/button_sprite_sheet.png', 193, 71);
            },
            create (phaser) {
                let self = this;
                console.log(self, phaser, this.game)
                phaser.game.stage.disableVisibilityChange = true;
                phaser.game.stage.backgroundColor = this.colorRecord
                /*
                phaser.input.onDown.add(function () {
                    self.colorRecord = Phaser.Color.getRandomColor(50, 255, 255);
                }, this);
                */

                var sprite = this.game.add.sprite(this.game.world.centerX, this.game.world.centerY, 'block')
                console.log(this.game.world.children[0])
                sprite.anchor.set(0.5)
                sprite.inputEnabled = true
                /*
                sprite.input.onDown.add(function() {
                    console.log('click sur le sprite')
                })
                */
                sprite.events.onInputDown.add(function() {
                    console.log('sprite down')
                })

                this.text = this.game.add.text(250, 16, 'Not clicked', { fill: '#ffffff' });
                sprite.events.onInputDown.add(this.listener, this.game)
                this.sprites.sprite = sprite
            },
            listener (e) {
                console.log('listener', e)
                this.text.text = 'you clicked !'
            },
            update (phaser) {
                phaser.game.stage.backgroundColor = this.colorRecord;
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
        },
        watch (val) {
            this.$nextTick(() => {
                // Besure update frame in every data change
                this.game.update()
            })
        }
    }
</script>