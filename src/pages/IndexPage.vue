<template>
  <q-page class="row items-center justify-evenly" ref="page">
    <q-resize-observer @resize="onResize" />
  </q-page>
</template>

<script lang="ts">
import { Todo, Meta } from 'components/models';
import { defineComponent, ref, toRaw } from 'vue';
import * as THREE from 'three';
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader.js';
export default defineComponent({
  name: 'IndexPage',
  components: {},
  setup() {
    //
    return {
      cubeGlb:
        'data:application/octet-stream;base64,Z2xURgIAAAAoIQAAgAQAAEpTT057ImFzc2V0Ijp7ImdlbmVyYXRvciI6Iktocm9ub3MgZ2xURiBCbGVuZGVyIEkvTyB2MS42LjE2IiwidmVyc2lvbiI6IjIuMCJ9LCJzY2VuZSI6MCwic2NlbmVzIjpbeyJuYW1lIjoiU2NlbmUiLCJub2RlcyI6WzBdfV0sIm5vZGVzIjpbeyJtZXNoIjowLCJuYW1lIjoiQ3ViZSJ9XSwibWF0ZXJpYWxzIjpbeyJkb3VibGVTaWRlZCI6dHJ1ZSwiZW1pc3NpdmVGYWN0b3IiOlsxLDEsMV0sImVtaXNzaXZlVGV4dHVyZSI6eyJpbmRleCI6MH0sIm5hbWUiOiJNYXRlcmlhbCIsInBick1ldGFsbGljUm91Z2huZXNzIjp7fX1dLCJtZXNoZXMiOlt7Im5hbWUiOiJDdWJlIiwicHJpbWl0aXZlcyI6W3siYXR0cmlidXRlcyI6eyJQT1NJVElPTiI6MCwiTk9STUFMIjoxLCJURVhDT09SRF8wIjoyfSwiaW5kaWNlcyI6MywibWF0ZXJpYWwiOjB9XX1dLCJ0ZXh0dXJlcyI6W3sic2FtcGxlciI6MCwic291cmNlIjowfV0sImltYWdlcyI6W3siYnVmZmVyVmlldyI6NCwibWltZVR5cGUiOiJpbWFnZS9qcGVnIiwibmFtZSI6Im9iYW1hIn1dLCJhY2Nlc3NvcnMiOlt7ImJ1ZmZlclZpZXciOjAsImNvbXBvbmVudFR5cGUiOjUxMjYsImNvdW50IjoyNCwibWF4IjpbMSwxLDFdLCJtaW4iOlstMSwtMSwtMV0sInR5cGUiOiJWRUMzIn0seyJidWZmZXJWaWV3IjoxLCJjb21wb25lbnRUeXBlIjo1MTI2LCJjb3VudCI6MjQsInR5cGUiOiJWRUMzIn0seyJidWZmZXJWaWV3IjoyLCJjb21wb25lbnRUeXBlIjo1MTI2LCJjb3VudCI6MjQsInR5cGUiOiJWRUMyIn0seyJidWZmZXJWaWV3IjozLCJjb21wb25lbnRUeXBlIjo1MTIzLCJjb3VudCI6MzYsInR5cGUiOiJTQ0FMQVIifV0sImJ1ZmZlclZpZXdzIjpbeyJidWZmZXIiOjAsImJ5dGVMZW5ndGgiOjI4OCwiYnl0ZU9mZnNldCI6MH0seyJidWZmZXIiOjAsImJ5dGVMZW5ndGgiOjI4OCwiYnl0ZU9mZnNldCI6Mjg4fSx7ImJ1ZmZlciI6MCwiYnl0ZUxlbmd0aCI6MTkyLCJieXRlT2Zmc2V0Ijo1NzZ9LHsiYnVmZmVyIjowLCJieXRlTGVuZ3RoIjo3MiwiYnl0ZU9mZnNldCI6NzY4fSx7ImJ1ZmZlciI6MCwiYnl0ZUxlbmd0aCI6NjQ2NywiYnl0ZU9mZnNldCI6ODQwfV0sInNhbXBsZXJzIjpbeyJtYWdGaWx0ZXIiOjk3MjksIm1pbkZpbHRlciI6OTk4N31dLCJidWZmZXJzIjpbeyJieXRlTGVuZ3RoIjo3MzA4fV19ICCMHAAAQklOAAAAgD8AAIA/AACAvwAAgD8AAIA/AACAvwAAgD8AAIA/AACAvwAAgD8AAIC/AACAvwAAgD8AAIC/AACAvwAAgD8AAIC/AACAvwAAgD8AAIA/AACAPwAAgD8AAIA/AACAPwAAgD8AAIA/AACAPwAAgD8AAIC/AACAPwAAgD8AAIC/AACAPwAAgD8AAIC/AACAPwAAgL8AAIA/AACAvwAAgL8AAIA/AACAvwAAgL8AAIA/AACAvwAAgL8AAIC/AACAvwAAgL8AAIC/AACAvwAAgL8AAIC/AACAvwAAgL8AAIA/AACAPwAAgL8AAIA/AACAPwAAgL8AAIA/AACAPwAAgL8AAIC/AACAPwAAgL8AAIC/AACAPwAAgL8AAIC/AACAPwAAAAAAAAAAAACAvwAAAAAAAIA/AAAAgAAAgD8AAAAAAAAAgAAAAAAAAIC/AAAAgAAAAAAAAAAAAACAvwAAgD8AAAAAAAAAgAAAAAAAAAAAAACAPwAAAAAAAIA/AAAAgAAAgD8AAAAAAAAAgAAAAAAAAIC/AAAAgAAAAAAAAAAAAACAPwAAgD8AAAAAAAAAgAAAgL8AAAAAAAAAgAAAAAAAAAAAAACAvwAAAAAAAIA/AAAAgAAAgL8AAAAAAAAAgAAAAAAAAIC/AAAAgAAAAAAAAAAAAACAvwAAgL8AAAAAAAAAgAAAAAAAAAAAAACAPwAAAAAAAIA/AAAAgAAAgL8AAAAAAAAAgAAAAAAAAIC/AAAAgAAAAAAAAAAAAACAPw86ED8gcRg+DzoQPyBxGD4POhA/IHEYPjP+ET/YRmc/M/4RP9hGZz8z/hE/2EZnPzDMpD6AYg4+MMykPoBiDj4wzKQ+gGIOPtrJpz8ww2Q/aiefPjDDZD9qJ58+MMNkP7STTz+A5tk9tJNPP4Dm2T20k08/gObZPRhmUj9gZ1w/GGZSP2BnXD8YZlI/YGdcPxr/iD9AycU9mPGPPUDJxT0a/4g/QMnFPQgdiD+441k/CB2IP7jjWT940IE9uONZPwEADgAUAAEAFAAHAAoABgATAAoAEwAXABUAEgAMABUADAAPABAAAwAJABAACQAWAAUAAgAIAAUACAALABEADQAAABEAAAAEAP/Y/+AAEEpGSUYAAQEAAAEAAQAA/9sAhAAKBwgWFhQYFhYWGRkYGhohHBoaGRwhHhwcHhweGhweIRwcHCQuJRweKx8cGiY4JisvMTU1NRokO0A7ND8uNDUxAQwMDBAPEB8SEh40KyQrNDQ0NDQ0NDQ0NDQ0NDQ0NDQ0NDQ0NDQ0NDQ0NDQ0NDQ0NDQ0NDQ0NDQ0NDQ0NDQ0NDT/wAARCAECAMMDASIAAhEBAxEB/8QAHAAAAgMBAQEBAAAAAAAAAAAAAAUDBAYCBwEI/8QAPBAAAQMCAwUFBwIFBQADAAAAAQACEQMhBDFBBRJRYXEGIoGRoQcTMrHB0fAUQiNicoLhM1KSsvEVJKL/xAAZAQADAQEBAAAAAAAAAAAAAAAAAQIDBAX/xAApEQACAgEDAwMDBQAAAAAAAAAAAQIRAxIhMQRBURMiYTJCgRQjcZHw/9oADAMBAAIRAxEAPwD2ZCEIAEIQgAQhCABRvqBoJJAAzJMAdSVi+1Hb+lQllCK1UGCQf4bDf4nDNwj4RzkheSdoO0eJxL5qVi8Xhk7rG89wWMcTKVgetbf9puCw8tY413jSn8Pi828pXnu3PabjKxik4UW6BgG8ernSfKFhaoOQBvx58OVl8/RuIF7aiYPLz0QMYYvtRi6pO/ia5MZe8cB4hpAKpN2vWEfxqs8d99ukH8hVXsg3nxXMN9AgC+zbmJ0xNeeVV8/9kwwvbPHsBazF1o03n70X/nklISy0t0z48l8qUi3pxP5kgDe7M9quPpn+I5lUfzsa3LmwNz4mVuNhe1rDVBGIYaLtS077epgBw10OWa8LeIsOAk8+S5Y+D90AfrDZW1qGIZv0KrKjeLDMciMweRTBfk7AbUqUnB9N7qbge65ji2OvEcjIXqXZD2quLm08cGwbCswR/wA2DTm3yzKBHr6FFSqte0Oa4OaRIcDIIORBFiFKmAIQhAAhCEACEIQAIQhAAhCjqPDQSSAAJJNgAMySgD5VqNa0ucQGgSSTAAHEnJeRdvO3VSoTRw53aJHefcOfPD/az1Kh7fdqXYomlReW0RlFi+/xH+UQYHjwjGYyDTaHQ2JuczBt1SbGiDE1yWgDujM6A2gxGsceCgw2Gh0ukyCc5sL68V2w2ZYAzJEWkXE6ZKZzd8tJdBNrCLWtAzJjXipspIrPAIzJEDu5weX3XzFVjuBrtLg6zr1tx4rumWgb0WtAGfG/jCr1J3SYuT5kzKLHRUe0uieHnw66Lk0CT4T4QmO5LblRV6UAN6C3PihSE4lIPjTMQfDXqvpfGlp1GXEAqy2hEuIn6c7qs6mSRy04BNMTRCx8EfVWDSue7JNwDlBnIDPUeCg3bLr3hJmdI/wOCYqPmQy8VJhqnevkbFfKLQQQT+f+r64Ed2P/ABAUbXsb27rYF4Y6amGcbsJu2RM0ybA8W5Hkbr3fZO1aWJpirReHtPDMHg4ZtPIr8sCpYRnlB85Trsx2jrYGv7yk4lkw+mTZ7bEiNHAZO0QB+nUJV2f23RxlBtai6WuzH7mu1DhoQmqYgQhCABCEIAEIQgDkmM14x247bOxDzRou3cM0w5wP+oWnMnMNBFhrY8hsPatts4fBFjDD653JzhkS8x07v9y8NosJlzT3RbkTrn1SY0i0C4g3z1AgjOOuqDQnvEyARMjQZSFNhsKXQGmZMwdbcslp9lbBF3ObfnlPILnnlUTox4XIybqRJsCRx85y8VxuO3mxJ3b+V/HJb6n2faQQRY+cKzT7PsBBjLhyWXro2eB+TzWphzIiRIM52v8AgUraUgDh816W/YtMghzQeEqKlsJgABA8vkn66F6DPPK2zSGh+hgAHMz4lU8RSc1038R5L06rs9oEBvmkuL2WS/KRztolHP5HLp/BgqsjKROn2XPu4IvM52/Cte3YpLnGLDX7IobBGbpP0haevEj9PIx5oCJnL84qANExx+a2uJ2CyLC/ollbYZmQR0P0TjnixPp5Iz7LKf3ZLfHjFv8A0/NOTsqDooMTs6AYsOH51VLNFkvC0Kd0xGvhn1UjXzYg6EQpPdkBcPpfvExw6fRaKRk40O+yfaapgK4qtM03mKtOfibPD/cATBtwylfo/BYtlWmypTcHMe0OaRkQRIX5Qc8S4xpBvmLXXqnsX7SkOdgXnumX0J0Iu9g4giXjo5UiGeyIQhMQIQhAAhCqbRxjaNJ9V3wsa5x4w0EwOJsgDxv2o473+OdS3oZh6bR/e6Huz5OZ/wAVk8NG6IkNLob4ZmOfFFN7sRiHveAHVHOe8m8b7i49IV3B0O9ujIZE5dT14LGctjbHG2OtjYBo0vEC2S1lKgAAEu2TTgcZ/LJ2xl1583bPQgqR8YyAg9FP7uV9NEIoqyo4TmPJfQ06Kd1NDGIoLKlVhg2VJzCYkBOHsJVVzJ04pNFKQrfSCrbkWTGsDJVUsuEhlOszRLMQxOqrVQrU5QmDFBC4eycwrdVii3VqjKSFmIw17pW9kAtPh5LS4hlikuJY2fn5LogzlyIUVG902zBg+Vl1sbaT8PWp1mfHTeHDnGYPIiR4lWqdIub6pZVaR0mfzkuiLs5pKj9ZYDGNrUmVWGWvaHNPJwkK0sT7JsWX7NpNOdNzmG82Di5vQBrmgdFtlZAIQhAAsH7W8eaeCZTbnVqtaRMd1suPUSGgjgSt4vJvbK8uq4Vk2Ac4DQuJa0dNUmBjdmURvF8xIAPDdFz5n5JnhsMzfkEmYMDmP/FX2TTIDnOsIkAZxy4yI53TrBMhxsJtYWA/zdcmR7nZiiOcC2BldNWASltC35KasauQ6kTMbyU24uKZUwKtCZBUpqFzOat1FGYToEyvKjfTtZWHBc17jmpKTE+JaVW3CfqrmJ1EqiXweSg0CpTVGoxX3vsqTznaEAK8QxVQVcxLswqZWkTORzVKS4psvzTaqYCV4ijJHMfJbxZzzRAxgyAkyMvD0uqeNwx3SRrE/P7JowbpnIjOOH58lzi2y0gZFs+Of0W0Wc8kbz2E4w7uJoE2Hu6jRr3g5rvDus8166vD/YpVDcbUYc30TH9j2z/2XuC3MWfUIQgR8XlXtcoh1ak4/tp5dXn0svVV5L7XaxbiaA0dTIPTeP3SfA1yJcFS7pcPiAEDSDBJn0TPZVE3L+PDPhH5qquzXuhodawgAD8+llpsBhxuAZkalcE5bnfjjSBtKALeWQTKmVA92Q/CpWrA3LDZ0Xa+MbKmFOypEtkT2yFHMKwGFRVKadCTRBv3hfH5KN7ZdK+OJhKyqKuLZGSoBqcFkhU6zAk0XYvqqnVOivvaCqlVikdi3EszhLnJpiGZpZWZCuJEmQVFxiKNh+Zrtympumy1TMpKxZE66R8gucSO6+Bfytr6qaswMcJyJUNV+9vETe1tRYXW8Gc00P8A2PsP/wAjMZUak8iXU/svd14v7HgDjaxiN2kY6F4ueq9oXQjnYIQhMR8XkftqY4VMM8Zbrx1Ic0x6r1xYL2u7PNTBtqAS6lUB/tcC0+u75JPga5M32Ypb7WvkkxHp9Fp6QgQFl+xdXeodLfdadhXmT+pnqQXtR8qHIKzRCrEy7PIKrj9stpGJnpGfX/CUIthOSRo6ULp1QBYWt24a02YXjUghRP7bttLXN4wJz4Rn5roWNnO8iN6505KJ5ssLT7d08iPBx73kMkyw3aJrxYx+cVMo0XGSZoGNB81XeIUODxUld4w7oKzdUbIsNcNyUurPHELn9YG0zPFZ7aW1YBLSjnYT9oxq1wNVSq4kTmspitsuGp/OSVVdrVHGARlK1jhbMJZ0jce9YTmq2IANwV5/UxdUmN4k8l1R2hVabOPzV+h8krqPg11Rq+UnJJg9syQHwNJGX+E2Y7PzWcoOPJrGalwW6tPebkJGSRPJaXA/mSf0XpFicPvVXN6/L/xXi5M8y2N/7G6YNau8aU2A/wBznR/0Xra8z9k1BtDDYivVe1odUDd5zoAbTbrNgd57vTgtZge1uFrVNym9xvAcWuDXHgHEepgHQldVpHJpb4RoUL4vqok8+2xtd9TFupNLg1tu64tg+GZPNQYrFVmtdQqzVw9RhaZ+Nh0Id+6DBg+a6920Yut/V8yU8fQsHRb933Xmqc9TdnqyhBRSrakef9i2lvvWOza/0Nh8itYDCq4rBtp4kvbbfpgnq1x+6nJ7p4qZPVKwiqjSEHaTaDmNDGSXON44LP4HY1V5LnNzveHHpmJWyds5peXvvo0aAD680p2ht9jJDYAFt6N4uPBg165LWM6VIxlBydsp1OzBIlxaBPw5elko2h2ba3N7Y4HPwMpyMFisTh31WP3AGOLGE7z3EXEjJvQLC7bNN3uvde93w0+9NQ/vtYCdO9e2YW8VJq7oxaSdJWXm7Lpggb7Z0v8AdaTZezCIAdnxWf2Ns9rqTnudAkbocZmAJtreVosJh302Me0jcIBIDpLWkwHRoOM6XWGS/J04orwbTZWBLIm/yVzaGH7qj2bVkC8jQ8Va2g6GrFVTNHdowm2K5aN0JPhaYcYcbJlt03lZmSXC5jUzaBxTirQ5DevhKBMZkZ7t466DxVCu3CN+LfMAklrS6wzJIyHNWdlUi97d+k/3ck7jIE5QXE/FzHNPe02JfAfhmbj/AHRpvY9gjdMkFkGA4SR4roxxXdnLlT+1GTdjsDuw3e5ktI8yVE+lQeO5YHr9UvwWzajZYW7rXxvucMg0zAPOFf2hhb7zGlv+6bA+Ct0uGSoy7oX4nZMfDZMNnP7oac226hVsHUcLOFp106H7ptSY3MRPmolJ1TKjFJ2i1RKW45rzXO5q0TyTJi7w9GXveRYNa0dbk/RRGVWaSjqohoYR5axjnucxhO4wnutLiXOIbkCS5xnmuNvNdTcwtcYEGxjNOaFKTvTlpwS/tRT7k/yNPqQlbb3NopJo9r7PYv3uFo1NXUxPUCD6goWW7D4s/oKHRw8nuCF0KbOGWD3MiFBzcVX3rEukdJEehWoBhg6a/VVO0OGh7HjUween28lT2liCJE5Bc0lok0dS/djF/wC2F+0ajffMaHAkNcLHSWx9VJTCTUKZNYvnIfMp3SWN3uauKi6KWLpuf3YEeJ9MlTZsFgfvkXGpWjNMcF890clSsmyjT2W2JBAPEW+RCV7Q2KLud7vjlPq45rQ/pZ5L47ZjMyN7+oz81orqiW1dswbsPeGjf6CGpvs/Z4uXMaHEEWFyCIgnUQn/AOhk2AA0VhmFDRldTJUVqsrbPoFovwXe1Hyy2isOGip4u4IUJbMrlmE2m6SRzScUjvJvtZsPPNV2NEhOLpDktyxhMQ8CyuOxNrzc3N1CyjqLH5r455n89VblsSkfHlk3aT1VTFu3hDW+QhNWM1IXRoDVKMmKSszRwys0qMDomGKwwAVaICbdkVRASpMXi9ygCBJNSOWU38AoiNVNjaYOGeT+0hw8In0lOI/B1s7HF823Z4ZL72lcfcNPHueu8PmUnwWI3VoqeG/UOw1ECQ+qCf6QBveknwTjzRo6W/Y9B7F7FDMDQBmSze/5kvHo5C1rGwAALCwQu3Qjznnk3yUdtUppOIzb3h4XPpKyW1yZJ0InzW8cJCxW0acb1M5sJg8W6ei5uqj3N+jlvpYrwFPul3H/AAmGFS7Z5c1xY4fECQRpZX8JmVyLg68n1MZ0RZStaBwGqioFdvMLSL2MWtyRzwo3eage8jRfWv001TseksthRvchrx0UOIxAyCHwNIie66q4kwCrBEqniWyCFkaoxW23S9Uqbk02xQJMpE95Flcd0KfI7w1WYVg0w4LP4DHO3i14gaLQYc90XlDVCjuibDWsSrrmgi2iWl8a3UtPEwkNohxYuldQpliH7w7whLK2aa4IlyQOv5qbaL4w7hxt5x9FCDcKxjMM57ABoZjiqRPcz2CYXQAvTfZxg97EFxuKTD4OeQB6Byxf6YUnAMbL3fCBkDkfXReydjNjHDYcNd/qPO8/qch4D1lbYo3KyeomlD+TRIQhdh5p8SDtFgd6Hjofofp5J8o69MOaWnUEKJxUotF45uMk0ZIUrtA0Kp0rPI5piam5IcIcLQqFTDvY4F4guuBqATEHgeXNebTPS1IuMfrqpN6VAx2X5mpZREYPC4BiSV07L7pdj8XAgZlU3SHFXsfamILnbrfHoFYZhjmVFgqO4wPIuc0txfacglrKL3kcAAPMlSl5Le/Bo6FIEHoleJEEpbsvbD6m9vU303DR4EEciD6L5jsbEyU3xRMU7sobRcwSXHp1WeZQ33clX2zjnE2BJ0b+ZBfdm1qmT9xg4iSVUYtRFKXuONo4WBIsRdW9iY8nuOzC4xAL7NO9z0VjAYUMcHeaG1Q1yN309VWeSOCb1aY3ZGoslOJChDZWrOsc1Ue66lrO45qAqyDml8Sb4Aguc3pHWEow93WzyC2b+wuJp1ZpFj2Ogy5265pgSCIuAdR5KlGT4Rm5xi93Qz7J7HY+v71wn3Q7oI/c6b+EE+IW/CUdndlGhS3XEOcTLiMpyAE6Qm67ccdMThyy1SPqEIVmYIQhAEZYJmBI1i6S9p6Msa7/AGmPA/5AT1U9qUd+k4cpHUXHyWeSNxaLxyqSZk2usFKHqsz4V0HLzUz1CStUtmlmHoGtVz7jfiPHkvm0q5Baxt3uIA8U0wtMU2BgN83HidUfUyrpF8gRGiXvwYDt5h3VZNUAZqs+vey1qzLVRztMjcHEarPfo21SRMHj/hN8a/u5pVs94355x4pN7lR4F2N2SxmV+ZzSx+DAMkArS7XKQPqi48kdw43JKLbGPBdEwqja8XXL8SIKWkNaHGCxdtxx/pPEcOoVTHGD6Jex5c0gfELtPAhWRU95Ta/LiOBFiPNJqmNSsqPMrh9vJTPaAVBWVIljzsVgve4yk2LNO+7oy4//AFujxXtSwfsw2du031yLuO6z+ltyRyLjH9i3i7cUajfk8/PK5V4PqEIWpkCEIQAIQhAAvi+oQBh61HcqPYdDbobj0IXDGJz2lw3w1RpZ3TQ+dvEJMKmq8zJHTJo9PFPVBMWbv/3BOTaZI6yG/IlSivL3cjHJdYobuID9HMLfkfos/jsLiHv7j9wF17ZDlzUxNJb/ANGnvHDmq1XaFNn7pPAXSHaWFxAAO+Xti/HxiB6KvTpPI7pb5FaR3Fpoa4naLX2DXAcSb+Spl7GN35NjkqdTA4j4g5kf0n7rmthahZukCeM/dW4LuCl8EOM2y55sAAlz8WR+1cVtj1BO8+OQVGpgiM3vt/MU1GJMpS8Fx+0GfuBCq1doMj4tdVUfhHPtJ8SrWG2QyLi6bUImful2GOErB0QfJM9it7lVugfbxAPzKW0Whg6JvgHBmGLz+8lx+Q9FlM1gqKOIMGFDRpOe9rG3c5wa0cS4gAeZUDq0mVuPZnsb3lV2If8ADTMNHF5Gf9rT5uHBPHFydGeWSirPSNl4IUaNOk3JjQ2eJAufEyfFXEIXoLY85uz6hCEACEIQAIQhAAhCEARVqQc0tcJBEFYXFUjSe6m68ZHiDkVvkp23swVmSLPb8J4/ynkfRYZsepWuUbYMmiVPhmPxDt5oB+JpVmg0Qqj2GbiCDBB0IzBUtMwAQuBHolr3aqYnZ7Cd4CDy1TBrwQuHkQrToBYabQN1xHjZUarYMi4g/VX8VTkpVXwpvfNXrsSdC3GPBM8vz6JU+mDpKcDDQPFV3UxklrB2+xSbTtkunNgdFLVfwVWoSQUrsGqK1epvGBqu8VjC9rabfhbY81Wc2F3gcK+o9lOm3ee87rW8TnnwABJPAFVVmeoYbB2S/FV20aY5vdoxurj9BqV7psvZ7KFJlKmIa0RzJ1J4km5S7sr2eZg6IYO8916j4+J30aMgPuny68cNK35OLLk1Pbg+oQhamQIQhAAhCEACEIQAIQhAAhCEAZ3tFs2QazB3gO8B+4DXqPl0WdoVAvQ1idubO9xU32juPNv5XcOnBcfUYq9y/J2dPl+1/ggexcEmLZqSm5dFoXMlZ1WU3NfwVWuXAfDyzV41CDE+iqVhM8D9E6CxJiC4nrmq7qf+E2xTBFsyqVSBl4pUXaKbqKXV3ZgK7ia8COaU1KmaqKZEmiCpmvX/AGfdl/01P31Rv8d4yP7G5ho5mxPlosv7N+zXv6n6mq3+Gx38MEfG8a82t+fQr11deKHdnDmyXsj6hCF0HOCEIQAIQhAAhCEACEIQAIQhAAhCEACq4zCtqMLHCQR5cCOYVlCTV7DTowGNwrqDt12R+Fwyd9jxC5ZXAW4xuDZVYWPEg/kg6FYraPZ2vTncHvGaR8QHNuvguSWFxdx4OuGZNVLkhfUbmqNeuCqGIxhaS1wLTkQ4QfEHJLquK5rJpmykho+t4JXjcSAOaiqY0QoGbNxFb/TpVH82tO7/AMjb1TjBsUppC7FYkk52Tnsd2bfjat5FBh77uOR3GniR5DwWk2D7MnOh+LdujP3VMyejn6dG+a9H2fgKdCm2nSYGtbkG/XieZXTDF5OaebwS4XDspsaxjQ1rQGtaBAAGQCnQhbnOCEIQAIQhAAhCEACEIQAIQhAAhCEACEIQAIQhAAhCEAV8ThmO+JjXdQD81l9tbLoBriKNIHjuN+yEKGXEZ9n9nUW02ltKmDAuGNB84TxfUKlwS+T4vqEJiBCEIAEIQgAQhCABCEIAEIQgD//ZAA==',
      width: ref(1920),
      height: ref(1080),
      scene: ref<THREE.Scene>(),
      camera: ref<THREE.PerspectiveCamera>(),
      renderer: ref<THREE.WebGLRenderer>(),
      killed: ref(false)
    };
  },
  mounted() {
    this.setupThreeJS();
  },
  beforeUnmount() {
    this.killed = true;
  },
  methods: {
    setupThreeJS() {
      //
      // Create a new scene
      this.scene = new THREE.Scene();
      this.camera = new THREE.PerspectiveCamera(
        72,
        this.width / this.height,
        0.1,
        1000
      );
      this.renderer = new THREE.WebGLRenderer();
      this.renderer.setSize(this.width, this.height);
      ((this.$refs.page as any) .$el as HTMLElement).appendChild(this.renderer.domElement);

      // Load GLTF from GLB
      var loader = new GLTFLoader();
      // load from dataURI
      var cubeObject = null as any;
      loader.load(this.cubeGlb, (gltf) => {
        if (!this.scene) {
          return;
        }
        this.scene.add(gltf.scene);
        cubeObject = gltf.scene.children[0];
      });


      this.camera.position.z = 5;

    // Render the scene
    var render = () => {
      if (!this.renderer || !this.scene || !this.camera) {
        return;
      }
      if (this.killed) {
        return;
      }
      requestAnimationFrame(render);

      // cube.rotation.x += 0.1;
      if (cubeObject != undefined) {
        cubeObject.rotation.y += 0.01;
        cubeObject.rotation.x += 0.001;
      }

      this.renderer.render(toRaw(this.scene), toRaw(this.camera));
    };

    render();
    },
    onResize(size: {width: number, height: number}) {
      this.width=  this.width
      this.height = size.height
    }
  },
});
</script>
