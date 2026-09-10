# Dashboard Layout Components

These components are reusable helpers for building responsive dashboards.

They are currently inside:

```text
src/components/layout/
```

## Components

### DashboardScaleFrame

Use this when your dashboard has a fixed design size, for example `1366 x 768`,
but it needs to fit the available browser, preview, or TV screen.

```vue
<DashboardScaleFrame :width="1366" :height="768" fit-mode="contain">
  <div class="dashboard-canvas">
    Dashboard content here
  </div>
</DashboardScaleFrame>
```

Props:

| Prop | Type | Required | Default | Use |
| --- | --- | --- | --- | --- |
| `width` | Number | Yes | - | Original dashboard design width |
| `height` | Number | Yes | - | Original dashboard design height |
| `fit-mode` | String | No | `contain` | How dashboard should fit screen |

Fit modes:

| Mode | Behavior | Best for |
| --- | --- | --- |
| `contain` | Keeps aspect ratio and fits inside screen | Normal browser/mobile |
| `stretch` | Fills width and height exactly | TV/fullscreen preview |

Use `contain` when you want to keep the original aspect ratio.

Use `stretch` when the TV screen must be filled completely with no empty space.

## DashboardGrid

Use this instead of writing CSS grid again and again.

```vue
<DashboardGrid
  columns="repeat(4, minmax(0, 1fr))"
  tablet-columns="repeat(2, minmax(0, 1fr))"
  mobile-columns="1fr"
  gap="10px"
>
  <div>Card 1</div>
  <div>Card 2</div>
  <div>Card 3</div>
  <div>Card 4</div>
</DashboardGrid>
```

Props:

| Prop | Type | Default | Use |
| --- | --- | --- | --- |
| `columns` | String | `repeat(12, minmax(0, 1fr))` | Desktop columns |
| `tablet-columns` | String | same as `columns` | Columns below `992px` |
| `mobile-columns` | String | tablet or desktop columns | Columns below `768px` |
| `gap` | String | `12px` | Desktop gap |
| `tablet-gap` | String | same as `gap` | Gap below `992px` |
| `mobile-gap` | String | tablet or desktop gap | Gap below `768px` |
| `full-height` | Boolean | `false` | Makes grid height `100%` |

## DashboardCard

Use this for dashboard panels with a common header and body style.

```vue
<DashboardCard title="Live Energy Sources" icon="fas fa-bolt">
  <LiveEnergy />
</DashboardCard>
```

Props:

| Prop | Type | Default | Use |
| --- | --- | --- | --- |
| `title` | String | empty | Card header title |
| `icon` | String | empty | Icon class before title |

You can also pass a custom title:

```vue
<DashboardCard icon="fas fa-chart-line">
  <template #title>
    Custom Title
  </template>

  Card content here
</DashboardCard>
```

## Basic Setup

Import the components:

```js
import DashboardCard from './layout/DashboardCard.vue';
import DashboardGrid from './layout/DashboardGrid.vue';
import DashboardScaleFrame from './layout/DashboardScaleFrame.vue';
```

Register them:

```js
export default {
  components: {
    DashboardCard,
    DashboardGrid,
    DashboardScaleFrame,
  },
};
```

## Recommended Usage

```vue
<DashboardScaleFrame
  :width="1366"
  :height="768"
  :fit-mode="isTV || isFullscreen ? 'stretch' : 'contain'"
>
  <div class="dashboard-canvas">
    <DashboardGrid columns="repeat(4, minmax(0, 1fr))" gap="8px">
      <!-- top cards -->
    </DashboardGrid>

    <DashboardGrid columns="300px minmax(0, 1fr)" gap="12px" full-height>
      <div class="left-column">
        <DashboardCard title="Live Energy Sources" icon="fas fa-bolt">
          <LiveEnergy />
        </DashboardCard>
      </div>

      <div class="right-column">
        <DashboardCard title="LTM Trends" icon="fas fa-chart-line">
          <YTDTrends />
        </DashboardCard>
      </div>
    </DashboardGrid>
  </div>
</DashboardScaleFrame>
```

## Canvas CSS

The content inside `DashboardScaleFrame` should fill the stage.

```css
.dashboard-canvas {
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
  gap: 8px;
  height: 100%;
  overflow: hidden;
  padding: 8px;
  width: 100%;
}
```

## Simple Rule

Use:

```text
DashboardScaleFrame -> controls screen fitting
DashboardGrid       -> controls row/column layout
DashboardCard       -> controls card look
```
