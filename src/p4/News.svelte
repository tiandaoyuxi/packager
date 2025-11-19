<script>
  import {_} from '../locales/';
  import Section from './Section.svelte';
  const color = '#b117f8';
</script>

<style>
  .badge {
    background-color: #b117f8;
    border-bottom: 2px solid #6f0073;
    color: white;
    padding: 2px 5px;
    border-radius: 4px;
  }
</style>

<Section accent={color}>
  <div>
    <p>
      <span class="badge">{$_('p4.newsTitle')}</span>
    </p>
    <p>
      {$_('p4.newsDescription')}
    </p>
  </div>
</Section>
