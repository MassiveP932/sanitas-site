<script>
  import { Router, Route } from "svelte-navigator";
  import Top from "./Header/Top.svelte";
  import Header from "./Header/Header.svelte";
  import Index from "./Home/Index.svelte";
  import Footer from "./Footer/Footer.svelte";
  import TreatmentsGroupIndex from "./TreatmentsGroupPage/TreatmentsGroupIndex.svelte";
  import TreatmentDetailIndex from "./TreatmentPage/TreatmentDetailIndex.svelte";
  import AboutIndex from "./About/AboutIndex.svelte";
  import ContactIndex from "./Contact/ContactIndex.svelte";
  import RestService from "$services/rest";
	import Modal from 'svelte-simple-modal';
	import { modal } from '$services/store';

  import {
    lang,
    langs,
    groups,
    treatments,
    general,
    translate,
    features,
  } from "$services/store";
  import FaqIndex from "./FAQ/FaqIndex.svelte";
  import DetoxDetailIndex from "./Detox/DetoxDetailIndex.svelte";
  const getGroups = async () => {
    let response = await RestService.getGroups(undefined, undefined, $lang);
    groups.set(response["groups"]);

  };
  getGroups();
  const getFeatures = async () => {
    let response = await RestService.getFeatures($lang);
    features.set(response["features"]);
  };
  getFeatures();
  const getTreatments = async () => {
    let response = await RestService.getTreatments($lang, undefined, true);
    treatments.set(response["treatments"]);

  };
  getTreatments();
  const getGenerals = async () => {
    let response = await RestService.getGenerals(undefined, undefined, $lang);

    general.set(response["generals"][0]);
  };
  getGenerals();
  const getTranslates = async () => {
    let response = await RestService.getTranslates($lang);
    translate.set(response["translates"][0]);
  };
  getTranslates();
  const getLangs = async () => {
    let response = await RestService.getLangs();
    langs.set(response["langs"]);
  };
  getLangs();

</script>
<Modal show={$modal} />

<div class="flex flex-col justify-between">
  <div>
    <Top />
    <Header />
  </div>
<div class="min-h-screen h-full">
  {#if $groups  && features}
  <Router>
    <Route path="home" component={Index} />
    <Route path="about" component={AboutIndex} />
    <Route path="contact" component={ContactIndex} />
    <Route path="faqs" component={FaqIndex} />
    <Route path="detox" component={DetoxDetailIndex} />
    <Route path="treatments/:treatment" component={TreatmentDetailIndex} />
    <Route path="departments/:group" component={TreatmentsGroupIndex} />

    <Route path="" component={Index} />
  </Router>
  {/if}
</div>
<Footer/>
</div>
