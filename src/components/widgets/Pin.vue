<template>
	<div class="col">
		<b>Enter Pin:</b>
		<slot></slot>
		<!--<p>
			Your pin is 4-12 characters long<br />
			<i>Please make sure you have set it within the web platform interface.</i>
		</p>-->
		<label class="pinWrapper" style="margin-bottom:10px;">
			<div class="inputLabel" style="opacity:1">{{pinTitle}} *</div>
			<input class="pin" :class="{'show':form.showPin.val1}" maxlength="1" :type="inputType" :placeholder="0" ref="pinCode1" @touchend="focusInput('val1', 1, $event)" @keyup="focusInput('val1', 1, $event)" v-model="form.pinCode.val1" />
			<input class="pin" :class="{'show':form.showPin.val2}" maxlength="1" :type="inputType" :placeholder="0" ref="pinCode2" @touchend="focusInput('val2', 2, $event)" @keyup="focusInput('val2', 2, $event)" v-model="form.pinCode.val2" />
			<input class="pin" :class="{'show':form.showPin.val3}" maxlength="1" :type="inputType" :placeholder="0" ref="pinCode3" @touchend="focusInput('val3', 3, $event)" @keyup="focusInput('val3', 3, $event)" v-model="form.pinCode.val3" />
			<input class="pin" :class="{'show':form.showPin.val4}" maxlength="1" :type="inputType" :placeholder="0" ref="pinCode4" @touchend="focusInput('val4', 4, $event)" @keyup="focusInput('val4', 4, $event)" v-model="form.pinCode.val4" />
			<input class="pin" :class="{'show':form.showPin.val5}" maxlength="1" :type="inputType" placeholder="" ref="pinCode5" @touchend="focusInput('val5', 5, $event)" @keyup="focusInput('val5', 5, $event)" v-model="form.pinCode.val5" />
			<input class="pin" :class="{'show':form.showPin.val6}" maxlength="1" :type="inputType" placeholder="" ref="pinCode6" @touchend="focusInput('val6', 6, $event)" @keyup="focusInput('val6', 6, $event)" v-model="form.pinCode.val6" />
			<input class="pin" :class="{'show':form.showPin.val7}" maxlength="1" :type="inputType" placeholder="" ref="pinCode7" @touchend="focusInput('val7', 7, $event)" @keyup="focusInput('val7', 7, $event)" v-model="form.pinCode.val7" />
			<input class="pin" :class="{'show':form.showPin.val8}" maxlength="1" :type="inputType" placeholder="" ref="pinCode8" @touchend="focusInput('val8', 8, $event)" @keyup="focusInput('val8', 8, $event)" v-model="form.pinCode.val8" />
			<input class="pin" :class="{'show':form.showPin.val9}" maxlength="1" :type="inputType" placeholder="" ref="pinCode9" @touchend="focusInput('val9', 9, $event)" @keyup="focusInput('val9', 9, $event)" v-model="form.pinCode.val9" />
			<input class="pin" :class="{'show':form.showPin.val10}" maxlength="1" :type="inputType" placeholder="" ref="pinCode10" @touchend="focusInput('val10', 10, $event)" @keyup="focusInput('val10', 10, $event)" v-model="form.pinCode.val10" />
			<input class="pin" :class="{'show':form.showPin.val11}" maxlength="1" :type="inputType" placeholder="" ref="pinCode11" @touchend="focusInput('val11', 11, $event)" @keyup="focusInput('val11', 11, $event)" v-model="form.pinCode.val11" />
			<input class="pin" :class="{'show':form.showPin.val12}" maxlength="1" :type="inputType" placeholder="" ref="pinCode12" @touchend="focusInput('val12', 12, $event)" @keyup="focusInput('val12', 12, $event)" v-model="form.pinCode.val12" />
		</label>

		<!-- Transfer Funds Button -->
		<div class="row" :class="{'justifyCenter':!enableCancelButton}">
			<ButtonWidget style="margin:10px;" v-on:click.native="transfer()" :buttonTxt="submitButtonTxt"/>
			<ButtonWidget v-if="enableCancelButton" style="margin:10px;" class="cancel" v-on:click.native="cancel()" buttonTxt="Cancel" />
		</div>
		<!-- xTransfer Funds Button -->
	</div>
</template>

<script>
import ButtonWidget from '@/components/widgets/ButtonWidget.vue';

/**
 * @description
 * <p>Pin interface</p>
 */
export default {
	name: 'Pin',
	components: {
		ButtonWidget,
	},
	data() {
		return {
			//form modal
			form: {
				pinCode: {
					val1: this.val[0],
					val2: this.val[1],
					val3: this.val[2],
					val4: this.val[3],
					val5: this.val[4],
					val6: this.val[5],
					val7: this.val[6],
					val8: this.val[7],
					val9: this.val[8],
					val10: this.val[9],
					val11: this.val[10],
					val12: this.val[11],
				},
				showPin: {
					val1: true,
					val2: true,
					val3: true,
					val4: true,
					val5: false,
					val6: false,
					val7: false,
					val8: false,
					val9: false,
					val10: false,
					val11: false,
					val12: false,
				},
			},
		};
	},
	props: {
		/**
		 * @model input value
		 */
		val: {
			type: Array,
			default: () => [
				'0',
				'0',
				'0',
				'0',
				'',
				'',
				'',
				'',
				'',
				'',
				'',
				'',
			],
		},
		pinTitle: {
			type: String,
			default: 'User Pin',
		},
		/**
		 * submit button text
		 */
		submitButtonTxt: {
			type: String,
			default: 'Initiate Transfer',
		},
		/**
		 * Enable Cancel button
		 */
		enableCancelButton: {
			type: Boolean,
			default: true,
		},
		/**
		 * Set Pin Type [text, password]
		 */
		inputType: {
			type: String,
			default: 'text',
		},
	},
	created() {
		const self = this;
		//update type to number for easy mobile keypad display
		if (self.$store.getters.whichPlatform === 'mobile') {
			self.form.inputType = 'number';
		}
		if (self.inputType === 'password') {
			self.form.pinCode.val1 = '';
			self.form.pinCode.val2 = '';
			self.form.pinCode.val3 = '';
			self.form.pinCode.val4 = '';
		}
	},
	mounted() {
		const self = this;
		//focus on pinCode input
		//timeout required as interface not available
		setTimeout(function() {
			self.$refs.pinCode1.focus();
		}, 10);
	},
	methods: {
		/**
		 * Transfer
		 * Submit event to send pin
		 *
		 * @returns nothing
		 * @public
		 */
		transfer() {
			const self = this;
			const AC = self.form.pinCode;
			const completePinCode = AC.val1+AC.val2+AC.val3+AC.val4+AC.val5+AC.val6+AC.val7+AC.val8+AC.val9+AC.val10+AC.val11+AC.val12;
			self.$emit('submit-pin', completePinCode);
		},
		/**
		 * Cancel
		 * Exits pin mode
		 *
		 * @returns nothing
		 * @public
		 */
		cancel() {
			const self = this;
			self.$emit('submit-pin', false);
		},
		/**
		 * Focus Pin Inputs
		 * - enter submits Pin code
		 * - allows easy traversal accross inputs with keyboard commands
		 * - - backspace/left & right arrow key detection
		 *
		 * @param {string} val - data pinCode reference
		 * @param {number} refNext - input position reference
		 * @param {object} e - keyup event
		 * @returns nothing
		 * @public
		 */
		focusInput(val, refNext, e) {
			const self = this;
			const refNextItem = (refNext === 12) ? 1 : refNext+1;
			const refPrevItem = (refNext === 1) ? 12 : refNext-1;

			this.$emit('keypress');
			//enter
			if (e.which === 13) {
				self.transfer();
				return;
			}
			//backspace
			if (e.which === 8) {
				self.form.pinCode[val] = (refNext <= 4) ? 0 : '';
				if (self.inputType === 'password') {
					self.form.pinCode[val] = '';
				}
				refNext = (refNext === 1)?12:refNext-1;
				if (refNextItem >= 4) {
					if (refPrevItem >= 3) {
					//	console.log('x', self.form.pinCode[`val${refNextItem}`]);
					//	if (self.form.pinCode[`val${refNextItem}`].length === 0) {
							let step = refNextItem;
							for (step; step <= 12; step++) {
								self.form.showPin[`val${step}`] = false;
							}
						//} else {
						//	self.form.showPin[`val${refNextItem}`] = true;
						//}
					}
				}
				self.$refs['pinCode'+refNext].focus();
				return;
			}
			//arrow left
			if (e.which === 37) {
				refNext = (refNext === 1)?12:refNext-1;
				self.$refs['pinCode'+refNext].focus();
				return;
			}
			//arrow right
			if (e.which === 39) {
				refNext = (refNext === 12)?1:refNext+1;
				self.$refs['pinCode'+refNext].focus();
				return;
			}

			//spacebar
			if (e.which === 32) {
				return;
			}

			//number pressed
			if (Number.isFinite(Number(e.key))) {
				/**
				 * key-up event.
				 *
				 * @event key-up
				 * @type {string} input ref
				 * @type {string} value
				 */
				self.form.pinCode[val] = e.key;
				if (refNextItem >= 4) {
					if (refPrevItem >= 3) {
						if (self.form.pinCode[`val${refPrevItem}`].length === 0) {
							self.form.showPin[`val${refNextItem}`] = false;
						} else {
							self.form.showPin[`val${refNextItem}`] = true;
						}
					}
				}
				setTimeout(() => {
					self.$refs['pinCode'+refNextItem].focus();
				}, 10);
			} else {
				let inputVal = Number(e.target.valueAsNumber);
				if (inputVal >= 0) {
					inputVal = Number(String(inputVal).substr(String(inputVal).length - 1));
				} else {
					inputVal = 0;
				}
				self.form.pinCode[val] = inputVal;
				if (refNextItem >= 4) {
					if (refPrevItem >= 3) {
						if (self.form.pinCode[`val${refPrevItem}`].length === 0) {
							self.form.showPin[`val${refNextItem}`] = false;
						} else {
							self.form.showPin[`val${refNextItem}`] = true;
						}
					}
				}
				setTimeout(() => {
					self.$refs['pinCode'+refNextItem].focus();
				}, 10);
			}
		},
	},
};
</script>

<style scoped>
.justifyCenter {
	justify-content: center;
}

label {
	font-size:0.9em;
	color:#636363;
	display: flex;
	/*margin:10px 0px 0px 0px;*/
	margin:0px;
	position: relative;
}
label.show.error .inputLabel,
label.show.error input:focus {
	color:#ff8585;
}
label.show.error input {
	border-bottom: solid 1px #ff8585;
}
.fullWidth {
	display: flex;
	flex-grow: 1;
}
input {
	border:0px;
	/*border-bottom:solid 1.5px #30c1ea;*/
	border-bottom: solid 1px #ddd;
	background:#fff;
	color:#a5a5a5;
	padding:4px 8px;
	height: 32px;
	line-height: 32px;
	font-size:1em;
	flex-grow: 1;
	/*margin: 26px 16px 8px;*/
	color:#666;
	border-radius: 3px;
	cursor:text
}

.night .error input,
.light .error input {
	border-bottom: solid 1px #ff8585;
    color: #ff8585;
}

.night input:focus,
.light input:focus {
	border-bottom:solid 1.5px #30c1ea;
	color:#73b6fb;
}

.inputLabel {
	position: absolute;
	top: 5px;
    left: 10px;
	color:#757575;
	font-size:0.9em;
	opacity:0;
	transition: opacity 1s;
	flex-grow: 1;
}
input {
	margin: 26px 6px 8px;
}

label.show .inputLabel {
	opacity:1;
}

input[name="password"] {
	padding-right:30px;
}

.night .amountInput.coin.gold,
.light .amountInput.coin.gold,
.coin.gold {
	background-image:url('../../assets/images/coin_gold.png');
	background-repeat: no-repeat;
}

.night .amountInput.coin.silver,
.light .amountInput.coin.silver,
.coin.silver {
	background-image:url('../../assets/images/coin_silver.png');
	background-repeat: no-repeat;
}

.night .amountInput {
	background-color: #101219;
    border-bottom: solid 1px #444;
}
.light .amountInput {
	background-color: #fff;
	border-bottom: solid 1px #ddd;
}
.amountInput {
	/*padding-left:24px;*/
	flex-grow:1;
	display:flex;
	background-size:20px !important;
    background-position: 4px 6px !important;
	height: 32px !important;
    /*margin: 10px 4px 8px;*/
	margin: 26px 6px 8px;
	width: auto !important;
	border-radius: 3px;
}
.night .amountInput input {
	background:none;
}

.night input {
	background:#101219;
	border-bottom: solid 1px #444;
}


.amountInput input {
	background:transparent;
	/*border:0px;*/
	margin:0px;
	padding-left: 30px;
    width: 100%;
}
.faEye {
	position: absolute;
	bottom: 18px;
	right: 14px;
	cursor: pointer;
}
.pin {
	width:26px;
	margin:26px 2px 8px;
	text-align:center;
	display: none;
}
.pin.show {
	display:block;
}
.platformWeb.min .pin {
	width:20px;
	margin:26px 2px 8px;
	text-align:center;
}

.pinWrapper {
	border-radius: 16px;
	padding:16px 8px;
}

.light .pinWrapper {
	border:solid 4px #eee;
}

.night .pinWrapper {
	border:solid 4px #171820;
}
</style>
