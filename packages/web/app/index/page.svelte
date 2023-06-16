<script>
  import { onMount } from 'svelte';
  import pbToTypescript from 'pbts/core';
  import ClipboardJS from 'clipboard';
  import Navbar from '../../components/nav-bar.svelte';

  let src = `
syntax = "proto3";

service MyService {
    rpc MyMethod (MyRequest) returns (MyResponse);
}

message MyRequest {
    string path = 1;
    string request_name = 2; // 请求名称
}

message MyResponse {
    int32 status = 1;
}
  `;

  let dest = '';

  let selectedDefinition = '0';

  let isWarning = false;

  let keepCamelCase = true;

  let keepRequire = false;

  function onProtobuf() {
    const isDefinition = !!Number(selectedDefinition);
    dest = pbToTypescript.parseProto(
      'syntax = "proto3";' + src,
      {
        isDefinition: isDefinition,
      },
      {
        keepCase: !keepCamelCase,
      }
    );

    if (keepRequire) {
      dest = dest.replace(/\?:/gm, ':');
    }
  }

  onMount(() => {
    window.onerror = () => {
      isWarning = true;
    };
    onProtobuf();
    new ClipboardJS('.button');
  });
</script>

<Navbar current={0} />
<div id="container">
  <div class="col">
    <h3>Protocol buffer</h3>
    <textarea name="" bind:value={src} on:input={onProtobuf} />
    {#if isWarning}
      <span class="rightcorner warning">Invald Protobuf</span>
    {/if}
  </div>
  <div class="col">
    <div class="tool-bar">
      <select
        bind:value={selectedDefinition}
        on:change={onProtobuf}
        on:blur={onProtobuf}
        class="type-selector"
      >
        <option value="1">Typescript d.ts</option>
        <option value="0">Typescript File</option>
      </select>
      <label>
        <input
          type="checkbox"
          bind:checked={keepCamelCase}
          on:change={onProtobuf}
          on:blur={onProtobuf}
        />
        属性驼峰
      </label>
      <label>
        <input
          type="checkbox"
          bind:checked={keepRequire}
          on:change={onProtobuf}
          on:blur={onProtobuf}
        />
        替换可选为必选
      </label>
    </div>
    <textarea name="" id="typescript" bind:value={dest} />
    <span class="rightcorner button" data-clipboard-target="#typescript"
      >Copy to clipboard</span
    >
  </div>
</div>

<style>
</style>
