<template>
  <div v-shortkey="['ctrl', 'alt', 'h']" @shortkey="active = true">
    <!-- breadcrumb start -->
    <div class="breadcrumb-container">
      <div class="container">
        <ol class="breadcrumb">
          <li><i class="fa fa-home pr-10"></i><a href="home">Accueil</a></li>
          <li class="active">Recherche</li>
        </ol>
      </div>
    </div>
    <!-- breadcrumb end -->
    <!-- section -->
<section class="main-container">
  <div class="container">
    <div class="row">
      <!-- section start -->
      <section class="dark-translucent-bg" style="background-image:url(assets/images/poignee_de_main.png);">
        <div class="container">
          <div class="row justify-content-lg-center">
            <div class="col-lg-12">
              <h2 class="text-center mt-4"><span class="bold_6">Rassurez</span> vos acheteurs potentiels</h2>
              <div class="separator with-icon"><i class="fa fa-car bordered"></i></div>
            </div>
          </div>
        </div>
      </section>
      <!-- section end -->
    </div>
  </div>
</section>
<!-- section -->
<section class="container" v-if="!active">
  <div class="row justify-content-lg-center">
    <div class="col-lg-2"></div>
    <div class="col-lg-8">
      <div>
        <h4>Vous souhaitez tester Histovec ?</h4>
        <p> Le service Histovec est actuellement en <strong>test</strong>. L'ouverture de service grand public est prévue pour l'automne. </p>
        <p> Actuellement, les tests sont conduits avec un <strong> panel restreint d'usagers </strong> (particuliers, concessionnaires, garages).
            Le service sera élargi progressivement en fonction des retours.
        </p>
        <p> Pour faire partie du panel de testeurs, prenez contact avec l'équipe par mail: <strong> <a href="mailto:histovec@interieur.gouv.fr">histovec@interieur.gouv.fr</a> </strong></p>
        <p> Professionnels, vous êtes intéressés ? Prenez contact
        </p>
        <p>
          Voici un exemple de rapport que permet de générer Histovec :
        </p>
      </div>
      <div> <img src="assets/images/exemple_rapport_g.png" class="img-responsive" width="889" height="2628"></div>
    </div>
    <div class="col-lg-2"></div>
  </div>
</section>
    <!-- section -->
    <section class="main-container" v-if="active">
      <div class="container">
        <div class="row">
          <div class="col-md-12">
            <div class="col-md-1"></div>
            <div class="col-md-10">
              <div class="alert alert-warning alert-icon text-center" role="alert"><i class="fa fa-exclamation-triangle"></i> Seuls les véhicules ayant une plaque au format <strong>AA-123-ZZ</strong> sont consultables pour l'instant </div>
            </div>
          </div>
          <div class="col-md-12">
            <!-- tabs start -->
            <!-- ================ -->
            <!-- Nav tabs -->
            <ul class="nav nav-tabs style-2" role="tablist">
              <li :class="[{'in active' : type_personne === 'particulier'}]"><a class="clickable" @click="type_personne = 'particulier'"><i class="fa fa-user pr-10"></i>Particulier</a></li>
              <li :class="[{'in active' : type_personne === 'pro'}]"><a class="clickable" @click="type_personne = 'pro'"><i class="fa fa-building-o pr-10"></i>Entreprise</a></li>
            </ul>
            <!-- Tab panes -->
            <div class="tab-content">
              <div class="tab-pane" id="h2tab1" :class="[{'in active' : type_personne === 'particulier'}]">
                <div class="row">
                  <div class="col-md-12">
                    <span class="info_red txt-small-11" v-if="(status == 'failed') && (!checkFields)">* Veuillez renseignez les champs obligatoires</span>
                    <fieldset>
                      <legend><span class="color-default">Titulaire</span></legend>
                      <form role="form">
                        <div class="row">
                          <div class="col-md-4">
                            <div class="form-group has-feedback" :class="[{'has-error' : (nom === '' && status !== 'init')}]">
                              <label class="control-label">Nom de naissance <span class="info_red" title="Ce champ est requis.">*</span></label>
                              <input v-focus ref="nom" name="nom" required="required" @paste="onPaste" type="text" class="form-control" v-bind:value="nom" v-on:input="nom = $event.target.value.replace(/\t.*/,'')" tabindex="1">
                              <i class="fa fa-user form-control-feedback"></i> </div>
                          </div>
                          <div class="col-md-4">
                            <div class="form-group has-feedback" :class="[{'has-error' : (prenom === '' && status !== 'init')}]">
                              <label class="control-label">Prénom <span class="info_red" title="Ce champ est requis.">*</span></label>
                              <input type="text" required="required" class="form-control" v-model="prenom" tabindex="2">
                              <i class="fa fa-user form-control-feedback"></i> </div>
                          </div>
                          <div class="col-md-4">
                            <div class="form-group has-feedback" :class="[{'has-error' : (date_naissance === '' && status !== 'init')}]">
                              <label class="control-label">Date de naissance <span class="info_red" title="Ce champ est requis.">*</span></label>
                              <input type="text" required="required" class="form-control" placeholder="xx/xx/xxxx" v-model="date_naissance" tabindex="3">
                              <i class="fa fa-calendar form-control-feedback"></i> </div>
                          </div>
                        </div>
                      </form>
                    </fieldset>
                    <fieldset>
                      <legend><span class="color-default">Carte grise</span></legend>
                      <form role="form">
                        <div class="row">
                          <div class="col-md-6">
                            <div class="form-group has-feedback" :class="[{'has-error' : (plaque === '' && status !== 'init')}]">
                              <label for="input" class="control-label">Plaque d'immatriculation <span class="info_red" title="Ce champ est requis.">*</span></label>
                              <input type="text" required="required" class="form-control" id="input" placeholder="AA-555-AA" v-model="plaque" tabindex="4">
                              <i class="fa fa-drivers-license-o form-control-feedback"></i> </div>
                          </div>
                          <div class="col-md-6">
                            <div class="form-group has-feedback plan position_left" :class="[{'has-error' : ((!checkFormule) && status !== 'init')}]">
                              <label for="input" class="control-label">N° de formule <span class="info_red" title="Ce champ est requis.">*</span></label> <a @click="modal = true" class="clickable text-info btn-sm-link"><i class="fa fa-info-circle fa-lg"></i> </a>
                              <input type="text" required="required" class="form-control" placeholder="2013BZ80335" v-model="formule" tabindex="5">
                              <i class="fa fa-pencil-square-o form-control-feedback"></i> </div>
                          </div>
                          <div class="col-md-4" v-if="false">
                            <div class="form-group has-feedback plan position_left">
                              <label for="input" class="control-label">Date première mise en circulation <span class="info_red" title="Ce champ est requis.">*</span></label>
                              <input type="text" required="required" class="form-control" placeholder="XX/XX/XXXX" data-toggle="popover" data-placement="top" data-content="Date de première mise en circulation, telle qu'indiquée sur la carte grise" title="Date de première mise en circulation" data-original-title="1ère circulation" data-trigger="hover" v-model="date_prem_immat">
                              <i class="fa fa-pencil-square-o form-control-feedback"></i> </div>
                          </div>
                        </div>
                      </form>
                    </fieldset>
                    <div class="form-group">
                      <div class="col-xs-offset-5 col-sm-7">
                        <button @click="onSubmit" class="btn btn-animated btn-default btn-sm" tabindex="6">
                          <i class="fa" :class="[{'fa-search' : (status === 'init')},
                                    {'fa-spin fa-spinner' : (status === 'posting')},
                                    {'fa-exclamation-triangle' : (status === 'failed')}]"></i>Rechercher
                        </button>
                        <!--
                        <a href="#" class="btn btn-animated btn-default btn-sm pop" data-container="body" data-toggle="popover" data-placement="top" data-content="Le certificat de situation administrative (CSA) est un document délivré par le ministère de l'Intérieur contenant des éléments d'information sur la situation administrative d'un véhicule.<br>Le CSA détaillé fait apparaître l'ensemble des informations relatives à la situation du véhicule." data-original-title="CSA" title="CSA"> Imprimer CSA détaillé<i class="fa fa-print"></i> </a>--> </div>
                      </div>
                  </div>
                </div>
              </div>
              <div class="tab-pane" id="h2tab2" :class="[{'in active' : type_personne === 'pro'}]">
                <div class="row">
                  <div class="col-md-12"> <span class="info_red txt-small-11" v-if="status == 'failed'">* Veuillez renseignez les champs obligatoire</span>
                    <fieldset>
                      <legend><span class="color-default">Titulaire</span></legend>
                      <form role="form">
                        <div class="row">
                          <div class="col-md-6">
                            <div class="form-group has-feedback" :class="[{'has-error' : (raison_sociale === '' && status !== 'init')}]">
                              <label class="control-label">Raison sociale <span class="info_red" title="Ce champ est requis.">*</span></label>
                              <input name="raison_sociale" @paste="onPaste" type="text" required="required" class="form-control" v-bind:value="raison_sociale" v-on:input="raison_sociale = $event.target.value.replace(/\t.*/,'')" tabindex="1">
                              <i class="fa fa-user form-control-feedback"></i> </div>
                          </div>
                          <div class="col-md-6">
                            <div class="form-group has-feedback" :class="[{'has-error' : (siren === '' && status !== 'init')}]">
                              <label class="control-label">N° SIREN <span class="info_red" title="Ce champ est requis.">*</span></label>
                              <input type="text" required="required" class="form-control"  v-model="siren" tabindex="2">
                              <i class="fa fa-building-o form-control-feedback"></i> </div>
                          </div>
                        </div>
                      </form>
                    </fieldset>
                    <fieldset>
                      <legend><span class="color-default">Carte grise</span></legend>
                      <form role="form">
                        <div class="row">
                          <div class="col-md-6">
                            <div class="form-group has-feedback" :class="[{'has-error' : (plaque === '' && status !== 'init')}]">
                              <label for="input" class="control-label">Plaque d'immatriculation <span class="info_red" title="Ce champ est requis.">*</span></label>
                              <input type="text" required="required" class="form-control" id="input" placeholder="AA-555-AA" v-model="plaque" tabindex="3">
                              <i class="fa fa-drivers-license-o form-control-feedback"></i> </div>
                          </div>
                          <div class="col-md-6">
                            <div class="form-group has-feedback plan position_left" :class="[{'has-error' : (formule === '' && status !== 'init')}]">
                              <label for="input" class="control-label">N° de formule <span class="info_red" title="Ce champ est requis.">*</span></label> <a @click="modal = true" class="clickable text-info btn-sm-link"><i class="fa fa-info-circle fa-lg"></i></a>
                              <input type="text" required="required" class="form-control" placeholder="2013BZ80335" v-model="formule" tabindex="4">
                              <i class="fa fa-pencil-square-o form-control-feedback"></i> </div>
                          </div>
                          <div class="col-md-4" v-if="false">
                            <div class="form-group has-feedback plan position_left">
                              <label for="input" class="control-label">Date première mise en circulation <span class="info_red">*</span></label>
                              <input type="text" required="required" class="form-control" placeholder="XX/XX/XXXX" data-toggle="popover" data-placement="top" data-content="Date de première mise en circulation, telle qu'indiquée sur la carte grise" title="Date de première mise en circulation" data-original-title="15GB Storage" data-trigger="hover" v-model="date_prem_immat" >
                              <i class="fa fa-pencil-square-o form-control-feedback"></i> </div>
                          </div>
                        </div>
                      </form>
                    </fieldset>
                    <div class="form-group">
                      <div class="col-xs-offset-5 col-sm-7">
                        <button @click="onSubmit" class="btn btn-animated btn-default btn-sm" tabindex="5">
                          <i class="fa" :class="[{'fa-search' : (status === 'init')},
                                    {'fa-spin fa-spinner' : (status === 'posting')},
                                    {'fa-exclamation-triangle' : (status === 'failed')}]"></i>Rechercher
                        </button>                        <!--
                        <a href="#" class="btn btn-animated btn-default btn-sm pop" data-container="body" data-toggle="popover" data-placement="top" data-content="Le certificat de situation administrative (CSA) est un document délivré par le ministère de l'Intérieur contenant des éléments d'information sur la situation administrative d'un véhicule.<br>Le CSA détaillé fait apparaître l'ensemble des informations relatives à la situation du véhicule." data-original-title="CSA" title="CSA"> Imprimer CSA détaillé<i class="fa fa-print"></i> </a>--> </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>

          </div>
          <!-- tabs end -->
        </div>
      </div>
    </section>
    <!-- section end -->
    <div class="container">
      <div class="row"> </div>
    </div>
    <!-- debut modal -->
    <div v-if="modal">
      <transition name="modal">
        <div class="modal-mask">
          <div class="modal-wrapper">
            <div class="modal-dialog">
              <div class="modal-content">
                <div class="modal-header">
                  <button type="button" class="close" @click="modal = false"><span aria-hidden="true">&times;</span><span class="sr-only">Fermer</span></button>
                  <h6 class="modal-title">Information n° de formule</h6>
                </div>
                <div class="modal-body" style="height: 250px; overflow-y: auto;"> <img alt="Indication localisation numéro de formule : sous le numéro d'immatriculation ou dans la bande MRZ ou sur la première page de la carte grise" src="assets/images/n-formule.svg" class="img-responsive" style="margin: 0 auto;"> </div>
                <div class="modal-footer"> <a href="#" class="btn radius-30 btn btn-animated btn-default" @click="modal = false">Fermer <i class="fa fa-close"></i></a> </div>
              </div>
            </div>
          </div>
        </div>
      </transition>
    </div>
    <!-- fin modal -->
  </div>

</template>

<script>

import CryptoJS from 'crypto-js'
import Shake from 'shake.js'

export default {
  components: {
  },
  data () {
    return {
      modal: false,
      active: false,
      type_personne: 'particulier',
      nom: '',
      raison_sociale: '',
      prenom: '',
      date_naissance: '',
      plaque: '',
      siren: '',
      formule: '',
      status: 'init'
    }
  },
  directives: {
    focus: {
      inserted: function (el) {
        el.focus()
      }
    }
  },
  computed: {
    checkFormule () {
      return this.formule.match(/^\d{4}[a-zA-Z]{2}\d{5}$/)
    },
    checkFields () {
      return ((this.nom && this.prenom && this.date_naissance) || (this.raison_sociale && this.siren)) && this.plaque && this.checkFormule
    },
    currentMonthNumber () {
      var date = new Date()
      date = date.getFullYear() + '' + this.pad(date.getMonth() + 1, 2)
      console.log(date)
      return date
    },
    currentWeekNumber () {
      var instance = new Date()

      // Create a copy of this date object
      var target = new Date(instance.valueOf())

      // ISO week date weeks start on monday
      // so correct the day number
      var dayNr = (instance.getDay() + 6) % 7

      // ISO 8601 states that week 1 is the week
      // with the first thursday of that year.
      // Set the target date to the thursday in the target week
      target.setDate(target.getDate() - dayNr + 3)

      // Store the millisecond value of the target date
      var firstThursday = target.valueOf()

      // Set the target to the first thursday of the year
      // First set the target to january first
      target.setMonth(0, 1)
      // Not a thursday? Correct the date to the next thursday
      if (target.getDay() !== 4) {
        target.setMonth(0, 1 + ((4 - target.getDay()) + 7) % 7)
      }

      // The currentWeekNumber is the number of weeks between the
      // first thursday of the year and the thursday in the target week
      var currentWeekNumber = 1 + Math.ceil((firstThursday - target) / 604800000)
      return instance.getFullYear() + '' + currentWeekNumber
    },
    id () {
      return this.hash(this.raison_sociale + this.siren + this.nom + this.prenom + this.date_naissance + this.plaque + this.formule)
    },
    code () {
      return this.hash(this.plaque + this.formule + this.currentMonthNumber)
    },
    key () {
      return this.hash(this.plaque + this.formule)
    }
  },
  methods: {
    onPaste (evt) {
      let data = evt.clipboardData.getData('Text').replace(/\s*$/, '').split(/\t+/)
      if (data.length > 1) {
        if (evt.target.name === 'nom') {
          this.nom = data[0]
          this.prenom = data[1]
          this.date_naissance = data[2]
          this.plaque = data[3]
          this.formule = data[4]
        }
        if (evt.target.name === 'raison_sociale') {
          this.raison_sociale = data[0]
          this.siren = data[1]
          this.plaque = data[2]
          this.formule = data[3]
        }
      }
    },
    pad (n, width, z) {
      z = z || '0'
      n = n + ''
      return n.length >= width ? n : new Array(width - n.length + 1).join(z) + n
    },
    hash (string) {
      var hash = string
      hash = hash.normalize('NFD').toLowerCase().replace(/[^0-9a-z]/g, '')
      hash = CryptoJS.SHA256(hash).toString(CryptoJS.enc.Base64)
      hash = hash.replace(/\+/g, '-').replace(/\//g, '_')
      return hash
    },
    onSubmit () {
      this.status = 'posting'
      if (this.checkFields) {
        this.$router.push({name: 'report', params: {id: this.id, key: this.key, code: this.code}})
      } else {
        this.status = 'failed'
      }
    }
  },
  created () {
    let myShakeEvent = new Shake({
      threshold: 15,
      timeout: 1000
    })
    myShakeEvent.start()
    window.addEventListener('shake', () => { this.active = true }, false)
    if (!window.location.host.match(/(histovec.fr|.gouv.fr$)/)) {
      this.active = true
    }
    this.type_personne = this.$route.params.t || 'particulier'
  }
}
</script>

<style lang="scss" scoped>
</style>
