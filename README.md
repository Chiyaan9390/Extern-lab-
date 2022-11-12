<div class="col-md-6" style="overflow-y: auto;"  (cdkDropListDropped)="drop($event)">
        <div class="drop" [style.border-width.px]="imagens.length > 0 ? 0: 3">
          <div class="abc">
              <ngb-carousel style="outline: none;" id="carousel" #carousel *ngIf="imagens" (slide)="change($event)">
                <ng-template *ngFor="let imgIdx of imagens; let i = index" [id]="i" ngbSlide>
                  <div class="picsum-img-wrapper">
                    <img [src]="imgIdx.Imagem" style="border-radius: 8px;" class="img-responsive Images">
                      </div>                    
                </ng-template>              
              </ngb-carousel>            
          </div>
        </div>
        <div class="row">
          <div class="Upcard" *ngFor="let imgIdx of imagens | slice:1" >
            <img class="img-responsive" [src]="imgIdx.Imagem" style="width: 100%; height: 100%">
          </div>
        </div>
      </div>
      drop(event: CdkDragDrop<string[]>) {
     moveItemInArray(this.imagens, event.previousIndex, event.currentIndex);
   }

   dropBig(event: CdkDragDrop<string[]>) {
     moveItemInArray(this.imagens, event.previousIndex, 0);
   }
   <div class="col-md-6" style="overflow-y: auto;">
    <div class="drop" [style.border-width.px]="imagens.length > 0 ? 0: 3">
        <div class="abc" cdkDropList #dropList="cdkDropList" cdkDropListOrientation="horizontal"
            [cdkDropListData]="imagens" 
            [cdkDropListConnectedTo]="[dragList]" 
            (cdkDropListDropped)="drop($event)">
            <ngb-carousel data-interval="false" style="outline: none;" id="carousel" #carousel *ngIf="imagens"
                (slide)="change($event)">
                <ng-template *ngFor="let imgIdx of imagens; let i = index" [id]="i" ngbSlide>
                    <div class="picsum-img-wrapper" cdkDrag [cdkDragDisabled]="true">
                        <img [src]="imgIdx.Imagem" style="border-radius: 8px;" class="img-responsive Images">
                      </div>
                </ng-template>
            </ngb-carousel>
        </div>
    </div>
     <div class="row" cdkDropList #dragList="cdkDropList" cdkDropListOrientation="horizontal"
        [cdkDropListData]="imagens2" 
        [cdkDropListConnectedTo]="[dropList]" 
        (cdkDropListDropped)="drop($event)">
        <div class="Upcard" *ngFor="let imgIdx of imagens2" cdkDrag>
            <img class="img-responsive" [src]="imgIdx.Imagem" style="width: 100%; height: 100%">
          </div>
        </div>
    </div>
    drop(event: CdkDragDrop<string[]>) {
    if (event.previousContainer === event.container) {
      moveItemInArray(event.container.data, event.previousIndex, event.currentIndex);
    } else {
      transferArrayItem(event.previousContainer.data,
                        event.container.data,
                        event.previousIndex,
                        event.currentIndex);
    }
  }
  drop(event: CdkDragDrop<string[]>) {
    if (event.previousContainer === event.container) {
      moveItemInArray(
        event.container.data,
        event.previousIndex,
        event.currentIndex
      );
    } else {
      const imagen = { ...(event.container.data[event.currentIndex] as any) };
      const previousImagen = {
        ...(event.previousContainer.data[event.previousIndex] as any)
      };
      event.container.data.splice(event.currentIndex, 1, previousImagen);
      event.previousContainer.data.splice(event.previousIndex, 1, imagen);
    }
  }
