
<template>
    <iframe width="100%" height="100%"  :src="url" title=""></iframe>
</template>
<style>
.HelloWorldWidget{
  position: absolute;
	width:100%;
	height: 100%;
	overflow: hidden;
	text-align: left;
}

</style>

<script>
import DojoWidget from "dojo/DojoWidget";
import lang from "dojo/_base/lang";
import on from "dojo/on";
import touch from "dojo/touch";
import UIWidget from "core/widgets/UIWidget";

export default {
  name: "Label",
  mixins: [UIWidget, DojoWidget],
  data: function () {
    return {
      value: this.value,
      style: {},
      model: {},
      url: this.url,
    };
  },
  components: {},
  computed: {
    options () {
      return this.style.options
    }
  },
  methods: {

    getName () {
      return 'Iframe'
    },

    postCreate () {

      this._borderNodes = [this.domNode];
      this._backgroundNodes = [this.domNode];
      this._shadowNodes = [this.domNode];
      this._paddingNodes = [this.domNode];
      this._labelNodes = [this.domNode];
    },

    getCreateTemplates () {
      return [
        {
          "id" : "Iframe",
          "type" : "Iframe", // must be the same as tghe name used in the SymbolService
          "_type" : "Widget",
          "category" : "WireFrame",
          "subcategory" : "AAAAAA",
          "name" : "Iframe",
            "x" : 0,
            "y" : 0,
            "w" : 100,
            "h" : 30,
            "z" : 0,
            "props" : {
              "label" : "Iframe",
              "link" : "url"
            },
            "has" : {
              "backgroundColor" : true,
              "data": true,
              "border": true,
              "label" : true,
              "padding" : true,
              "advancedText" : true
            },
            "style" : {
              "fontSize" : 20,
              "fontFamily" : "Helvetica Neue,Helvetica,Arial,sans-serif",
              "textAlign" : "left",
              "letterSpacing" : 0,
              "lineHeight" : 1,
              "color": "#333333",
              "borderTopRightRadius" : 0,
              "borderTopLeftRadius" : 0,
              "borderBottomRightRadius" : 0,
              "borderBottomLeftRadius" : 0,
              "borderTopWidth" : 0,
              "borderBottomWidth" : 0,
              "borderRightWidth" : 0,
              "borderLeftWidth" : 0,
              "borderTopStyle" : "solid",
              "borderBottomStyle" : "solid",
              "borderRightStyle" : "solid",
              "borderLeftStyle" : "solid",
              "borderTopColor" : "#333333",
              "borderBottomColor" : "#333333",
              "borderRightColor" : "#333333",
              "borderLeftColor" : "#333333",
              "background": null
            }
        }
      ]
    },

    getDataProperties () {
      return [
        {
          label: "URL Link",
          type: "String",
          value: "add your link here",
          key: "link",
          isProp:false,
        },
      ]
    },

    wireEvents () {
      this.own(this.addClickListener(this.domNode, lang.hitch(this, 'onClick')));
      this.own(on(this.domNode, touch.over, lang.hitch(this, 'onDomMouseOver')));
      this.own(on(this.domNode, touch.out, lang.hitch(this, 'onDomMouseOut')));
    },

    getLabelNode () {
      return this.$refs.lblNode;
    },

    render (model, style, scaleX, scaleY) {
      this.model = model;
      this.url = ""
      this.style = style;
      this._scaleX = scaleX;
      this._scaleY = scaleY;

      this.setStyle(style, model);
      if (model.props && model.props.undefined  ) {
        this.url = model.props.undefined;
        this.setValue(model.props.undefined);
      }
    },

    getValue () {
      return this.value;
    },

    setValue (value) {
      this.value = value;
    },

    getState () {
      return {
        type: 'value',
        value: this.value,
      };
    },

    setState (state) {
      /**
       * Hack for the time when we use the getValueLabel() mechnism!
       */
      if (this.hackValueLabel) {
        return;
      }

      if (state && state.type == 'value') {
        this.setValue(state.value);
      }
    },

    resize () {
    },

    onClick: function(e) {
      this.stopEvent(e);
      this.emitClick(e);
    }
  }
};
</script>